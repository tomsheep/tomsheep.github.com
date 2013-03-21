---
layout: post 
title: 面试题(2) 
category : 白银时代
date: 2010-04-13 04:42:41.000000000 +08:00
tags: [面试, Morgan Stanley, Java]
---

依然是周六摩根的笔试题，还是时间原因，比上一道更可惜，是到简单的高分题，给出两个已排序的array，设计算法找出将两个array合并以后的中位数。若算法为O(log(n))时间得满分，O(n)时间给半分。最后做时只剩5分钟，仓促地写了一个O(n)的傻瓜算法，就是mergesort的合并过程。
  
其实O(log(n))算法更加简单，当时着急想都没想。下面给一个解：

{% highlight java %}

import java.util.Arrays;

import java.util.Random;

class Median {
    private int [] arr1; 
    private int [] arr2;
    private int n;

    public Median(int n) {
        super ();
        Random rnd = new Random(); 
        this.n = n;
        arr1 = new int[n];
        arr2 = new int[n];
        for ( int i = 0; i &lt; n; i++) {
            arr1[i] = rnd.nextInt() % 10;
            arr2[i] = rnd.nextInt() % 10;
        }
        Arrays.sort(arr1);
        Arrays.sort(arr2);
    }

    // O(log(n))
    public int getMedain(int l1, int h1, int l2, int h2) {

        if (l1 &gt;= h1 || l2 &gt;= h2)
            return arr1[l1];

        int m1 = (l1 + h1) / 2;
        int m2 = (l2 + h2) / 2;

        if (arr1[m1] &lt; arr2[m2]) {
            return getMedain(m1, h1, l2, m2);
        } else { 
            return getMedain(l1, m1, m2, h2);
        }
    }

    public int median() { 
        return getMedain(0, n - 1, 0, n - 1);
    }

    public void printArrays() {
        System.out.println(" arr1: ");

        for ( int a : arr1) {
            System.out.print(a + " ");
        }
        System.out.println();
        System.out.println(" arr2: ");

        for ( int a : arr2) {
            System.out.print(a + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int n = 5;
        Median m = new Median(n);
        m.printArrays();
        System.out.println(" median: " + m.median());
    }
}
{% endhighlight %} 
