# 环境问题
 *version 1.0.0*


## 模块化打包,及环境配置

- 1 打包时存在外部资源如图片,css等,分为两种情况

  - 在代码中无引用的资源webpack不会编译这些资源
  
    > 需要放到asserts目录中，在使用这些资源时需要带有模块名如fansion-meta/asserts/aa.jpg,
    >　同时在调试模式下的express选项
                                                                 >
    ```
    server.use('/'+process.env.npm_package_name+'/asserts', express.static('./asserts'', {maxAge: 1000 * 60 * 60}))
    ```
    > 打包时需要通过CopyWebpackPlugin插件拷贝到lib目录中        
    
    ```
    plugins: [
        new VueLoaderPlugin(),
        new ProgressBarPlugin(),
        // copy custom static assets
        new CopyWebpackPlugin([
          {
            from: path.resolve(__dirname, '../asserts'),
            to: 'asserts/'
          }
    ])]
    ```                                             
  
  - 在代码中有引用如图片通过import imageUrl from '@static/abc.jpg'
  
      > 这些资源需要放在static目录下,并设置webpack打包选项
      ```
      //webpack
      output: {
         publicPath: 'fansion-meta/'
      }
      resolve: {
          alias: {
            '@static': resolve('static')
          }
      }
      ```
      在引用项目中在调时环境中设置webpack和express的选项
      

