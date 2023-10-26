# 控制器规范

## 1.控制器如何返回API数据
```php
$this->success('', [
    'list'   => $list,
    'total'  => 10,
]);
```


## 2.初始化, 设置uid和用户可见Uid
```php

protected array $child;
protected int $uid;


public function initialize(): void
{
    parent::initialize();
    $this->model = new \app\admin\model\Goods;

    $this->uid = $this->auth->getAdmin()->id;
    $this->child = (new Member())->getChildList($this->uid, $this->auth->isSuperAdmin());
}
```
- 用户模型中

```php
// 对外使用: 获取获取所有下级用户的ID数组
public function getChildList($uid, $isSuperAdmin = false)
{
    if ($isSuperAdmin && $uid == 1) {
        $result = $this->getAllUids();            
    } else {
        $result = $this->getChildIds($uid);
        $result[] = $uid; // 添加当前用户
    }
    return $result;
}

 // 无限极方法,获取所有下级用户的ID数组(只供内部使用)
private function getChildIds($uid)
{
    $subordinateIds = [];
    $subordinates = $this->where('pid', $uid)->select();
    foreach ($subordinates as $subordinate) {
        $subordinateIds[] = $subordinate->id;
        $subordinateIds = array_merge($subordinateIds, $this->getChildIds($subordinate->id));
    }
    return $subordinateIds;
}

/**
 * 获取所有用户id
 * @return array 最后返回的是一维数组, 由所有用户ID组成的
 */
private function getAllUids()
{
    $result = $this->where('id', '<>', 1)->column('id');
    return $result;
}
```