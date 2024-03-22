```c++
double getKthElement(vector<int>& nums1, vector<int>& nums2,int k){
        int len1=nums1.size(),len2=nums2.size();
        int idx1=0,idx2=0;//最左边到哪
        while(true){
            if(idx1==len1){
                return nums2[k+idx2-1];
            }
            if(idx2==len2){
                return nums1[k+idx1-1];
            }
            if(k==1){
                return min(nums1[idx1],nums2[idx2]);
            }
            int new_idx1 = min(len1-1,k/2-1+idx1);
            int new_idx2 = min(len2-1,k/2-1+idx2);
            if(nums1[new_idx1]<nums2[new_idx2]){
                k-=new_idx1-idx1+1;
                idx1=new_idx1+1;
            }
            else{
                k-=new_idx2-idx2+1;
                idx2=new_idx2+1;
            }
        }
    }
```
当数组1的k/2-1小于数组2的k/2-1时候,数组1的左边可以直接被抛弃!因为他们必不可能是所需要的数.
注意判断边界条件:
1. 如果k/2-1大于某个数组的长度,就查找最后一个
2. 如果k=1,比较两个的首位即可
3. 如果其中一个数组已经为空,直接返回剩下的一个数组的k即可

