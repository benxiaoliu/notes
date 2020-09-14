BIT explanation:
===
https://www.youtube.com/watch?v=v_wj_mOAlig

https://blog.csdn.net/Yaokai_AssultMaster/article/details/79492190?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param

315
======
(1) have a copy array, sort the copy array, so each element will have rank in range (0 to n - 1);
(2) In the BIT, the pathSum from the root (index 0) to node (index i) is the total number of currently scanned elements with rank less than or equal to i - 1;
(3) so when we solve problems like 315, we start scan the original array from the end, find its rank in the sorted array, supposed it is i, then we know all elements previously scanned with rank in range of (0 to i - 1) will count as smaller numbers after self for current num, so we calculate the path from node with index i to the root 0;
(4) since current num is with rank i, than then pathSum of (0 to k) with k >= i + 1 need to be incremented by 1, since pathSum of (0 to k) means total number of element with rank less than k...
Similar for cases (493):
* The only different is that for searching we require nums[i] > 2 * nums[j], so all the previous scan value satisfy the condition need to have val < num / 2, so we need to find the rank of num/2, and count the pathSum from 0 - rank in the BIT.

