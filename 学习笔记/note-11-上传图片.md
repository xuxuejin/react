#### 1.react 开发图片上传功能

在文件中引用：

  import upload from '../../utils/uploadImg'

  <div className="upload-wrap">
      <input className="file-btn" type='file' accept='image/*' ref="titlePic" onChange={upload.bind(this, 'titlePic')} />
      <input type="button" className="button bg-blue" id="image2" value="+ 浏览上传" />
  </div>
  
uploadImg文件

import axios from 'axios';
import { Modal, message } from 'antd';
import setAuthorizationToken from './setAuthorizationToken';

let isUploading = false;
let imgBaseUrl = 'https://xxx.com/';

var uploadImg = {
    upload: function(imgObj) {
        // this 组件
        const _this = this;
        var file = _this.refs[imgObj].files[0];
        var formFile = new FormData();
        // 防止重复上传图片
        if(isUploading) {
            Modal.error({
                title: '图片正在上传，请稍等片刻~'
            })
        }

        if(!file) return;

        formFile.append("file", file)

        if (!/image\/[png|jpg|jpeg]/.test(file.type)) {       
            Modal.error({
                title: '只能上传JPG 、JPEG 、GIF、 PNG格式的图片~'
            })

            return false;
        } else if(file.size / 1024 / 1024 > 2) {
            Modal.error({
                title: '超过2M限制 不允许上传~'
            })

            return false;
        } else {
            isUploading = true;
            // loadding动画
            _this.setState({
                [imgObj]: 'https://xxxx.gif'
            })

            axios({
                method: 'post',
                headers: {
                    'Content-Type': 'multipart/form-data'
                },
                url: '/picture/upload',
                data: formFile
            }).then((res) => {
                if (res.data.code === 0) {
                    let img = new Image();
                    let timer = null;
                    
                    img.src = imgBaseUrl + res.data.data;

                    // 定时器 确保图片可以访问
                    clearInterval(timer)
                    timer = setInterval(() => {
                        if(img.complete) {
                            clearInterval(timer)
                            _this.setState({
                                [imgObj]: imgBaseUrl + res.data.data
                            })
                            message.success('图片上传成功！');
                        }
                    }, 300)                  

                } else {
                    message.error(res.data.message);
                }
                isUploading = false;
            }, error => {
                let data = error.response;
    
                if(data.status === 401) {
                    localStorage.clear();
                    setAuthorizationToken(false)
                    // token过期，跳转登录页面
                    window.location.href = '/'
                }
            }).catch(function (error) {
                isUploading = false;
                console.log(error)
            })
        }

    }
}

export default uploadImg.upload;
