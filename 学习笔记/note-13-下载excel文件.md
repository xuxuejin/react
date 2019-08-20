#### 1.前端提供下载 excel 功能

后台返回的网站访问统计数据，除了用 echart 可视化展示外，还需要提供可供下载的 excel 的功能：

1.直接填写下载链接

  <a href={`https://xxx/xxx/xxx?time=${time}`}>下载报表</a>
  
2.前端请求二进制文件，组装数据，生成一个下载链接

    <button type="button" onClick={() => this.downloadExcel()} >下载报表</button>

    downloadExcel = (startTime, endTime) => {
        http.post('/xxx/xxx', {
            startTime: startTime,
            endTime: endTime
        }).then((res) => {
            data是二机制文件
            this.getExcel(res.data)
        }, (error) => {
            console.log(error);
        })
    }
    
    getExcel = (data) => {
        let url = window.URL.createObjectURL(new Blob([data], {
            type: "application/vnd.ms-excel,charset=UTF-8"
        }));

        let link = document.createElement('a');

        link.style.display = 'none';
        link.href = url;
        link.download = `${this.state.endTime}.xlsx`;
        // link.setAttribute('download', `${this.state.endTime}.xls`);
        document.body.appendChild(link);
        link.click();
        //释放对象URL
        window.URL.revokeObjectURL(link);
    }

使用这种方法，在发送请求的时候，需要 responseType 为 "blob"，不然下载的 excel 文件打开时乱码或者直接就打不开了

    axios.post(api, params, {responseType:'blob'}).then(res => {
        resolve(res)
        reject(res)
    }, error => {
        let data = error.response;
    }).catch(function (error) {
        message.error('服务发生了故障，请稍后再试!')
    })



