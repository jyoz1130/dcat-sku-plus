fork for 个人定制使用

## 安装

#### composer安装

```shell
composer require jyoz1130/dcat-sku-plus

```

#### 发布资源
```shell
php artisan vendor:publish --tag=public
```

## 使用

```php
// app/Admin/Controllers/ProductController.php

protected function form()
{
    return Form::make(new Product(), function (Form $form) {
        $skuParams = [
            // 扩展第一列
            [
                'name'    => '会员价', // table 第一行 title
                'field'   => 'member_price', // input 的 field_name 名称
                'default' => 5, // 默认值
            ],
            // 扩展第二列
        ];
        // 新增时
        $form->sku('sku', '生成SKU')->addColumn($skuParams);
        
        // 编辑时
        $skuData = []; // 数据结构见附录
        $skuString = json_encode($skuData);
        $form->sku('sku', '生成SKU')->addColumn($skuParams)->value($skuString);
        
        // 获取提交的数据.
        $form->saving(function (Form $form) {
            // 拿到SKU数据，按照业务逻辑做响应处理即可。
            dd($form->input('sku'));
        });
    });
}
```

## 附录

#### 最终生成的SKU数据结构，仅供参考

```json
{
  "attrs": {
    "测试": [
      "测试1",
      "测试2"
    ],
    "颜色": [
      "绿",
      "黄"
    ],
    "含电池": [
      "含电池"
    ],
    "内存": [
      "8G"
    ]
  },
  "sku": [
    {
      "测试": "测试1",
      "颜色": "绿",
      "含电池": "含电池",
      "内存": "8G",
      "pic": [
        {
          "short_url": "sku/HjdrG0RpfIwI3kkgpNNFfmxPasgaOLg6bBPOCDxd.jpg",
          "full_url": "http://127.0.0.1:8000/storage/sku/HjdrG0RpfIwI3kkgpNNFfmxPasgaOLg6bBPOCDxd.jpg"
        },
        {
          "short_url": "sku/bkucABLjzRQ5pEHYX0gwdS1VxJrS6ObiQCIWVvIl.png",
          "full_url": "http://127.0.0.1:8000/storage/sku/bkucABLjzRQ5pEHYX0gwdS1VxJrS6ObiQCIWVvIl.png"
        }
      ],
      "stock": "",
      "price": "",
      "member_price": 5
    }
  ]
}
```