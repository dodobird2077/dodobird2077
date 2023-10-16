# 自定义添加页面, 带文件导入及文本框二选一


1. 首先禁用列表页的添加, b并增加自定义按钮
```html
    <TableHeader
        :buttons="['refresh', 'comSearch', 'quickSearch', 'columnDisplay']"
        :quick-search-placeholder="t('Quick search placeholder', { fields: t('transferLog.quick Search Fields') })"
    >
        
    </TableHeader>
```