1、采用 let param = new URLSearchParams()

把参数封装在param里面

param.append('username', 'admin'),

param.append('password', 'admin') ,

     sbt(){
            let param = new URLSearchParams()
             param.append('username', 'admin'),
             param.append('password', 'admin') , 
                      this.axios({
                                method: "post",
                                url: '/api/PsychoSys/regedit.action',
                                data: param
                            })
                            .then(function(res) {
                               console.log(res);
                           })
                            .catch(function(err) {
                               console.log(err);
                           });
     
          },

 ![](图片/20190510100602556.png)

2、引入 qs ，这个库是 axios 里面包含的，不需要再下载了

      sbt(){
            let data = {
             'username': 'ddd',
             'password': '101010'
            }
             
                      this.axios({
                                method: "post",
                                url: '/api/PsychoSys/regedit.action',
                                data: Qs.stringify(data)
                            })
                            .then(function(res) {
                               console.log(res);
                           })
                            .catch(function(err) {
                               console.log(err);
                           });
     
          },
![](图片/20190510101658650.png)