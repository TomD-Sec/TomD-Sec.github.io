---
title: "Blog: How does ChatGPT fair against leetcode?"
date: 2023-03-05T18:06:18+01:00
coverSize: partial
ThumbnailImage: images/leetcode-3521542-2944960.png
categories:
- blog

keywords:
- chatGPT
- leetcode
- Blog

---


I chose a difficult question, but one that is relatively common, merging linked-lists. I used the “Merge k Sorted Lists” problem in GoLang. 

![Image](<images/2023-02-08 19_01_12-Hello Chat Request..png>) 

 

    /** 

    * Definition for singly-linked list. 

    * type ListNode struct { 

    *     Val int 

    *     Next *ListNode 

    * } 

    */ 

    func mergeKLists(lists []*ListNode) *ListNode { 

        if len(lists) == 0 { 

            return nil 

        } 

        

        var mergeTwoLists func(*ListNode, *ListNode) *ListNode 

        mergeTwoLists = func(l1 *ListNode, l2 *ListNode) *ListNode { 

            if l1 == nil { 

                return l2 

            } 

            if l2 == nil { 

                return l1 

            } 

            if l1.Val < l2.Val { 

                l1.Next = mergeTwoLists(l1.Next, l2) 

                return l1 

            } else { 

                l2.Next = mergeTwoLists(l1, l2.Next) 

                return l2 

            } 

        } 

        

        interval := 1 

        for interval < len(lists) { 

            for i := 0; i < len(lists) - interval; i += interval * 2 { 

                lists[i] = mergeTwoLists(lists[i], lists[i + interval]) 

            } 

            interval *= 2 

        } 

        

        return lists[0] 

    }

 

![Image](<images/2023-02-08 19_03_01-.png>) 

 

We find that the solution is accepted easily with a pretty decent efficiency. In fact, the code almost looks identical to the initial structure provided by the problem.  

But now let’s see if I can get some more efficiency out of it. And perhaps see if it can explain what the code is doing, as it’s perfectly possible this code has been pulled directly from a write-up that could have been in ChatGPT’s training data. 

![Image](<images/2023-02-08 19_08_41-Hello Chat Request..png>) 

 

    /** 

    * Definition for singly-linked list. 

    * type ListNode struct { 

    *     Val int 

    *     Next *ListNode 

    * } 

    */ 

    func mergeKLists(lists []*ListNode) *ListNode { 

        // Check if the input lists is empty 

        if len(lists) == 0 { 

            return nil 

        } 

    

        // Helper function to merge two linked lists into one sorted list 

        var mergeTwoLists func(*ListNode, *ListNode) *ListNode 

        mergeTwoLists = func(l1 *ListNode, l2 *ListNode) *ListNode { 

            // If either list is empty, return the non-empty list 

            if l1 == nil { 

                return l2 

            } 

            if l2 == nil { 

                return l1 

            } 

    

            // Choose the node with the smaller value and recursively call 

            // mergeTwoLists on the remaining nodes 

            if l1.Val < l2.Val { 

                l1.Next = mergeTwoLists(l1.Next, l2) 

                return l1 

            } else { 

                l2.Next = mergeTwoLists(l1, l2.Next) 

                return l2 

            } 

        } 

    

        // Repeat the merging process until there's only one list left 

        interval := 1 

        for interval < len(lists) { 

            for i := 0; i < len(lists) - interval; i += interval * 2 { 

                lists[i] = mergeTwoLists(lists[i], lists[i + interval]) 

            } 

            interval *= 2 

        } 

    

        return lists[0] 

    } 



![Image](<images/2023-02-08 19_06_20-.png>) 

 

Interestingly... The new “More efficient” code is actually slower than that of the previous code, let’s see if ChatGPT can correct this as well. 

 

Rather nicely ChatGPT even remembers my initial prompt about writing comments to explain what’s going on as it writes the new code. The code is certainly of a much larger size than before, but beats the other two programs (and 99% of the other users) at a whopping 3ms runtime.  

 

    /** 

    * Definition for singly-linked list. 

    * type ListNode struct { 

    *     Val int 

    *     Next *ListNode 

    * } 

    */ 

    func mergeKLists(lists []*ListNode) *ListNode { 

        // Check if the input lists is empty 

        if len(lists) == 0 { 

            return nil 

        } 

    

        // Helper function to merge two linked lists into one sorted list 

        var mergeTwoLists func(*ListNode, *ListNode) *ListNode 

        mergeTwoLists = func(l1 *ListNode, l2 *ListNode) *ListNode { 

            // If either list is empty, return the non-empty list 

            if l1 == nil { 

                return l2 

            } 

            if l2 == nil { 

                return l1 

            } 

    

            // Choose the node with the smaller value and recursively call 

            // mergeTwoLists on the remaining nodes 

            if l1.Val < l2.Val { 

                l1.Next = mergeTwoLists(l1.Next, l2) 

                return l1 

            } else { 

                l2.Next = mergeTwoLists(l1, l2.Next) 

                return l2 

            } 

        } 

    

        // Repeat the merging process until there's only one list left 

        interval := 1 

        for interval < len(lists) { 

            for i := 0; i < len(lists) - interval; i += interval * 2 { 

                lists[i] = mergeTwoLists(lists[i], lists[i + interval]) 

            } 

            interval *= 2 

        } 

    

        return lists[0] 

    }

 

![Image](<images/2023-02-08 19_12_33-.png>) 

 

 

But now let’s throw another curveball... I’ve decided I no longer like Go and want to give, lets say Python (a weakly typed language with a vastly different syntax to Go) a try.  

![Image](<images/2023-02-08 19_17_25-Hello Chat Request..png>) 

 

And we hit our first roadblock, a syntax error let's see if ChatGPT can fix it by itself. 

![Image](<images/2023-02-08 19_19_08-.png>) 

![Image](<images/2023-02-08 19_21_05-Hello Chat Request..png>) 

 

The error persists, so I provide a more specific error message in my prompt. ChatGPT then makes the assumption I’m using Python3, so I need to correct it. After a polite reminder that we're using Python2 it provides me with the following:

![Image](<images/2023-02-08 19_24_42-Hello Chat Request..png>) 

 

    # Definition for singly-linked list. 

    class ListNode(object): 

        def __init__(self, val=0, next=None): 

            self.val = val 

            self.next = next 

            

    class Solution(object): 

        def mergeKLists(self, lists): 

            # Check if the input lists is empty 

            if not lists: 

                return None 

    

            # Create a priority queue to store the heads of each list 

            queue = [] 

            for node in lists: 

                if node: 

                    queue.append(node) 

    

            # Create a dummy node as the head of the result list 

            dummy = ListNode() 

            tail = dummy 

    

            # Pop the smallest node from the priority queue and add it to the result list 

            while queue: 

                node = min(queue, key=lambda x: x.val) 

                queue.remove(node) 

                tail.next = node 

                tail = tail.next 

                if node.next: 

                    queue.append(node.next) 

    

            return dummy.next 



![Image](<images/2023-02-08 19_25_54-Merge k Sorted Lists - LeetCode.png>) 

 

Success! This time it runs... albiet rather slowly at over 4600ms. Let’s try and squeeze some efficiency out of it. This time ChatGPT calls on the heapq library. 

 

    import heapq 

    

    # Definition for singly-linked list. 

    class ListNode(object): 

        def __init__(self, val=0, next=None): 

            self.val = val 

            self.next = next 

            

    class Solution(object): 

        def mergeKLists(self, lists): 

            # Check if the input lists is empty 

            if not lists: 

                return None 

    

            # Create a priority queue to store the heads of each list 

            queue = [] 

            for node in lists: 

                if node: 

                    heapq.heappush(queue, (node.val, node)) 

    

            # Create a dummy node as the head of the result list 

            dummy = ListNode() 

            tail = dummy 

    

            # Pop the smallest node from the priority queue and add it to the result list 

            while queue: 

                _, node = heapq.heappop(queue) 

                tail.next = node 

                tail = tail.next 

                if node.next: 

                    heapq.heappush(queue, (node.next.val, node.next)) 

    

            return dummy.next 



![Image](<images/2023-02-08 19_28_13-.png>) 

 

The code hits a runtime of 27ms, massively beating out the previous runtime of over 4600ms.  

 

 

Truly incredible, but of course programming is more than just solving interview questions. Rest assured that while ChatGPT can knock socks off with tasks like this, ultimately there needs to be someone who understands what is going on to guide the Ai, and to know what needs to be done and where. We won't all be on the dole just yet!

**And lastly, while I did use LeetCode for this, I would never recommend using ChatGPT on Leetcode for anything other than curiosity and experimenting. Ultimately the point of LeetCode is to learn - You'll not only be cheating the leaderboards, but yourself of learning oppurtunities if you chose to use ChatGPT for easy answers. You can’t bring ChatGPT into an in-person coding interview or an airgapped, secure work environment, so best hone those skills the old fashioned way!** 

