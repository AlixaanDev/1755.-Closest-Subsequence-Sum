# 1755.-Closest-Subsequence-Sum
class Solution:
    def minAbsDifference(self, nums: List[int], goal: int) -> int:
        def get_subset_sums(start, size): 
            sums = {0}
            for i in range(start, start + size):
                sums |= {nums[i] + prev for prev in sums}
            return sums 
           
        N = len(nums)
        left_len = (N // 2) + N % 2
        right_len = N - left_len
        left = get_subset_sums(0, left_len)
        right = sorted(get_subset_sums(left_len, right_len))
            
        min_diff = float(inf)
        for left_sum in left:
            target = goal - left_sum
            idx = bisect_left(right, target)
            if idx > 0:
                min_diff = min(min_diff, abs(target - right[idx - 1]))
            if idx < len(right):
                min_diff = min(min_diff, abs(target - right[idx]))
            if min_diff == 0:
                break
            
        return min_diff
