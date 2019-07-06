
 - 可以直接打印SQL

```
<?php

// hasOne() 中 where 多个的情况一般转化为 IN 查询
$model = AnnualCard::find()->where(['>', 'id', '0'])->one();
$obj = $model->hasOne(JekunUserCar::className(), ['car_license' => 'car_license', 'jekun_user_id' => 'jekun_user_id']);
// 打印执行的SQL语句
print_r($obj->createCommand()->getRawSql());die();
// print: SELECT * FROM `jekun_user_car` WHERE (`jekun_user_car`.`is_deleted`=0) AND ((`car_license`, `jekun_user_id`) IN (('粤A52225', 19)));



// 普通查询
$model = YyerpUserVoucher::find()->where(['inner_code' => "89"])->createCommand()->getRawSql();
// 打印执行的SQL语句
print_r($model);die();
// print: SELECT * FROM `yyerp_user_voucher` WHERE (`inner_code`='89') AND (`yyerp_user_voucher`.`is_deleted`=0);

?>
```

 - 可以控制打印格式是对象还是数组
 
 
1. 返回结果为数组：

```
$data = \Yii::$app->db->createCommand("SELECT * FROM yyerp_user_voucher WHERE inner_code = {$inner_code}")->queryAll();

        $queryStr = <<<SQL
SELECT * FROM jekun_user LEFT JOIN yyerp_jekun_user ON jekun_user.id = yyerp_jekun_user.jekun_user_id
WHERE yyerp_jekun_user.id is NULL AND jekun_user.is_deleted = 0 AND jekun_user.status = 1;
SQL;
        $list = \Yii::$app->db->createCommand($queryStr)->queryAll();
```

2. 返回结果为对象：

```
$data = YyerpUserVoucher::findBySql("SELECT * FROM yyerp_user_voucher WHERE inner_code = {$inner_code}")->one();
```



