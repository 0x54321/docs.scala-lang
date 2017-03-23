---
layout: overview-large
title: ���������������� ������������ ���������

disqus: true

partof: parallel-collections
language: ru
num: 7
---

## ������������ �����

������������ ��������� ������������� ����������� ������ ������� ������������ ����� ��� ���������� ��������. � ����� ���������� ������ ������������ ��������� ���� ��� ���������� ������ ������������ �����, ������� � �������� �� ������������ � ������������� �������� �� ����������.

������ ������ ������������ ����� �������� ������ �� ���������� ���� �������; ����� ���� �� ����������, ��� � ����� ������ ����������� �� ����� ������ ���������. ��������� � ���, ��� ��������� ���������� ���� �������, ����� ������ � ����������� ������ \[[1][1]\].

� ��������� ����� ��� ������������ ��������� �������� ��������� ���������� ������� ��������� �����. ��������, `ForkJoinTaskSupport` ���������� ����������� "fork-join" ���� � ������������ �� ��������� �� JVM 1.6 ��� ����� �������. ����� ����������� `ThreadPoolTaskSupport` �������� �������� ��� JVM 1.5 � ��� �����, ������� �� ������������ ���� "fork-join". `ExecutionContextTaskSupport` ����� ���������� ��������� ���������� �� ��������� �� `scala.concurrent`, � ���������� ��� �� ��� �������, ��� � `scala.concurrent` (� ����������� �� ������ JVM, ��� ����� ���� ��� "fork-join" ��� "thread pool executor"). �� ��������� ������ ������������ ��������� ����������� ������ ������������ ����� ��������� ����������, ������� ������������ ��������� ���������� ��� �� ��� "fork-join", ��� � API �������� "future".

������� ����� ������������ ����� ��� ������������ ��������� ����� ���:

    scala> import scala.collection.parallel._
    import scala.collection.parallel._
    
    scala> val pc = mutable.ParArray(1, 2, 3)
    pc: scala.collection.parallel.mutable.ParArray[Int] = ParArray(1, 2, 3)
    
    scala> pc.tasksupport = new ForkJoinTaskSupport(new scala.concurrent.forkjoin.ForkJoinPool(2))
    pc.tasksupport: scala.collection.parallel.TaskSupport = scala.collection.parallel.ForkJoinTaskSupport@4a5d484a
    
    scala> pc map { _ + 1 }
    res0: scala.collection.parallel.mutable.ParArray[Int] = ParArray(2, 3, 4)

����������� ���� ����������� ������������ ��������� �� ������������� "fork-join" ���� � ������� ������������ ������ 2. ��������� ��������� ������������ "thread pool executor" ����� ���:

    scala> pc.tasksupport = new ThreadPoolTaskSupport()
    pc.tasksupport: scala.collection.parallel.TaskSupport = scala.collection.parallel.ThreadPoolTaskSupport@1d914a39
    
    scala> pc map { _ + 1 }
    res1: scala.collection.parallel.mutable.ParArray[Int] = ParArray(2, 3, 4)

����� ������������ ��������� �������������, ���� ������� ������������ ����� ����������� �� �������������. ����� ������������ ��������� ����������������� �� ���������� ������������������ ����, ��� ���� ����������� �������� �� ���������-- ������ ������������ ����� ��������� ����������.

����� ����������� ����������� �������� ��������� �����, ���������� ��������� ����� `TaskSupport` � ����������� ��������� ������:

    def execute[R, Tp](task: Task[R, Tp]): () => R
    
    def executeAndWaitResult[R, Tp](task: Task[R, Tp]): R
    
    def parallelismLevel: Int

����� `execute` ��������� ����������� ���������� ������ � ���������� "future" � �������� ������ � �������� ���������� ����������. ����� `executeAndWait` ������ �� �� �����, �� ���������� ��������� ������ ����� ���������� ������. ����� `parallelismLevel` ������ ���������� �������������� ���������� ����, ������� ������������ ����� ���������� ��� ������������ �������.

## ������

1. [On a Generic Parallel Collection Framework, June 2011][1]

  [1]: http://infoscience.epfl.ch/record/165523/files/techrep.pdf "parallel-collections"
