```ts
// nsum通用解法核心方法
function nSumTarget(nums, n, start, target) {
  // 前提：nums要先排序好
  let res = [];
  if (n === 2) {
    res = towSumTarget(nums, start, target);
  } else {
    for (let i = start; i < nums.length; i++) {
      // 递归求(n - 1)sum
      let subRes = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
      for (let j = 0; j < subRes.length; j++) {
        res.push([nums[i], ...subRes[j]]);
      }
      // 跳过相同元素
      while (nums[i] === nums[i + 1]) i++;
    }
  }
  return res;
}

function towSumTarget(nums, start, target) {
  // 前提：nums要先排序好
  let res = [];
  let len = nums.length;
  let left = start;
  let right = len - 1;
  while (left < right) {
    let sum = nums[left] + nums[right];
    if (sum < target) {
      while (nums[left] === nums[left + 1]) left++;
      left++;
    } else if (sum > target) {
      while (nums[right] === nums[right - 1]) right--;
      right--;
    } else {
      // 相等
      res.push([nums[left], nums[right]]);
      // 跳过相同元素
      while (nums[left] === nums[left + 1]) left++;
      while (nums[right] === nums[right - 1]) right--;
      left++;
      right--;
    }
  }
  return res;
}
```

