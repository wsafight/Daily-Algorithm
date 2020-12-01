# 树组件查询

查询已经生成的树组件数据，以此来进行其他操作。

通过递归转循环提高效率

```ts
function arrayTreeFilter<T>(
  data: T[],
  filterFn: (item: T, level: number) => boolean,
  options?: {
    childrenKeyName?: string;
  }
) {
  options = options || {};
  options.childrenKeyName = options.childrenKeyName || "children";
  var children = data || [];
  var result: T[] = [];
  var level = 0;
  do {
    var foundItem: T = children.filter(function(item) {
      return filterFn(item, level);
    })[0];
    if (!foundItem) {
      break;
    }
    result.push(foundItem);
    children = (foundItem as any)[options.childrenKeyName] || [];
    level += 1;
  } while (children.length > 0);
  return result;
}
```