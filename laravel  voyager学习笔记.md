# laravel  voyager学习笔记


### 访问laraveld storage目录中的图片
  web默认情况只能访问public目录  
  在public中创建storage软连接到storage目录  
  两种方式  
    1. ln -s storage/app/public /public/storage  
    2. php artisan storage:link