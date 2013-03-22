---
layout: post 
title: 面试题(1) 
category : 白银时代
date: 2010-04-13 04:33:23.000000000 +08:00
tags: [面试, Morgan Stanley, Java]
---

周六摩根的笔试，状态一般，有一些应该拿分的题目时间太紧没来得及做，有点可惜。
  
下面是其中之一：给定两个String，找出后一个在前一个中的所有“顺序匹配”。举例如str1为“abcabc”，str2为“abc”，则所有匹配应为
  
> （0,1,2, (0,1,5), (0,4,5), (3,4,5). 要求给出非递归算法。
  
下面是回来后写的一个解，包含递归和非递归版本。其中非递归算法有些讨巧，其实就是把递归的操作用显式的栈来模拟，起码名义上满足了题目要求。
  
{% highlight java %}

import java.util.Stack;

public class StringMatch {
    private String input;
    private String cset;
    private int [] buf;
    private Stack<Solution> stack;

    public StringMatch(String input, String cset) { 
        super();
        this.input = input;
        this.cset = cset;
        buf = new int[cset.length()];
        stack = new Stack<Solution>();
    }

    public void recursiveMatch(int pos1, int pos2) {
        if (pos2 == cset.length()) {
        System.out.print("match: ");
        for(int i = 0; i < buf.length; i++) {
            System.out.print(buf[i] + " ");
        }
        System.out.println(); 
        return;
        }

        if (pos1 == input.length()) return;
        if (input.charAt(pos1) == cset.charAt(pos2)) {
            buf[pos2] = pos1;
            recursiveMatch(pos1 + 1, pos2 + 1);
        }
        recursiveMatch(pos1 + 1, pos2);
    }

    public void nonRecersiveMatch() {
        Solution sol = new Solution(0, 0, new int[cset.length()]);
        stack.push(sol);

        while(!stack.isEmpty()) {
            Solution cur = stack.pop();
            if (cur.pos2 == cset.length()) {
                System.out.print("match: ");

                for(int i = 0; i < cur.buf.length; i++) {
                    System.out.print(cur.buf[i] + " ");
                }
                System.out.println();
                continue;
            }

            if (cur.pos1 == input.length()) {
                continue;
            }

            if (input.charAt(cur.pos1) == cset.charAt(cur.pos2)) {
                cur.buf[cur.pos2] = cur.pos1;
                stack.push(new Solution(cur.pos1 + 1, cur.pos2 + 1, cur.buf.clone()));
            }
            stack.push(new Solution(cur.pos1 + 1, cur.pos2, cur.buf.clone()));
        }
    }

    public static void main(String[] args) {
        StringMatch nr = new StringMatch("abcabc", "abc");
        System.out.println("recursive");
        nr.recursiveMatch(0, 0);
        System.out.println("Non recursive");
        nr.nonRecersiveMatch();
    }

    private class Solution { 
        int pos1;
        int pos2;
        int [] buf;

        public Solution(int pos1, int pos2, int[] buf) {
            this.pos1 = pos1;
            this.pos2 = pos2;
            this.buf = buf;
        }
    }
}
{% endhighlight %}

当时时间利用有些失误，放掉了这道题。
 
