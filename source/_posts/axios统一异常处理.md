---
title: axios统一异常处理
date: 2018-09-10 15:44:19
tags:
---
在前端中我们经常会遇到接口异常的情况，可能是请求异常，或者是响应异常，这个时候需要友好的处理。对应响应来说，针对于不同的接口可能会对应返回不同的状态码，而有些异常状态码（如401，203）是共有的，并且处理操作还一样，那么统一放在一块处理甚好。
#### 介绍一下axios的interceptors
  > 拦截器，可以截取请求或响应,在then或catch处理前
 ```javascript
 // 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
 ```
 当然也可以随时移除拦截器，如下
 ```
     axios.interceptors.(request|response).eject(/*...*/)
 ```
#### 统一处理异常代码
```
axios.interceptors.response.use(response => {
  const data = response.data
  // 根据返回的code值来做不同的处理(和后端约定)
  switch (data.msg) {
  case '201':
    toast.error('系统异常，即将重新登录', {
      position: toast.POSITION.BOTTOM_CENTER,
      pauseOnHover: false,
      hideProgressBar: true,
    })
    sessionStorage.clear()
    Cookies.remove('name', { path: '' })
    history.push('/')
    break
  case '202':
  case '203':
  case '204':
  case '205':
    // 操作失败
    toast.error(data.result, {
      position: toast.POSITION.BOTTOM_CENTER,
      pauseOnHover: false,
      hideProgressBar: true,
    })
    break
  case '206':
  case '207':
  
    // 系统错误
    toast.error('系统异常，即将重新登录', {
      position: toast.POSITION.BOTTOM_CENTER,
      pauseOnHover: false,
      hideProgressBar: true,
    })
    sessionStorage.clear()
    Cookies.remove('name', { path: '' })
    history.push('/')

    break
  case '208':
  case '209':
  case '210':
  case '211':
  case '213':
  case '214':
  case '215':
  case '216':
  case '217':
  case '218':
  case '219':
    // 系统错误
    toast.error(data.result, {
      position: toast.POSITION.BOTTOM_CENTER,
      pauseOnHover: false,
      hideProgressBar: true,
    })

    break
  default:
    return response
  }
  return response
}, (err) => {
  // 这里是返回后端代码错误处理（500 etc.）
  toast.info(err.toString(), {
    position: toast.POSITION.BOTTOM_CENTER,
    pauseOnHover: false,
    hideProgressBar: true,
  })
  return Promise.resolve(err)
})
```
> VUE可以使用`import { Message } from 'iview';`代替`toast`

以上就差不多了。
> 补充：
> 因为之前我对axios的不'规范'书写（`axios.post(url, data, config`)）,简写方式也不是不能用，但是这样也缺少了扩展性，比如说以下代码：
> ```
export const postRequest = (url, params) => {
    let accessToken = getStore("accessToken"); // 获取token
    return axios({
        method: 'post',
        url: `${base}${url}`,  //base为统一前缀，也可以设置baseURL
        data: params,
        transformRequest: [function (data) { //允许在向服务器发送前，修改请求数据
            let ret = '';
            for (let it in data) {
                ret += encodeURIComponent(it) + '=' +  //encodeURIComponent 编码
                encodeURIComponent(data[it]) + '&';
            }
            return ret;
        }],
        headers: { // 设置请求头
            'Content-Type': 'application/x-www-form-urlencoded',
            'accessToken': accessToken //这里设置默认参数
        }
    });
};
```
> 这里网上摘抄的一段，注释以上。如果简写，那么这么精彩的配置，那就不适合了。
> 介绍一下上文的`transformRequest`,
> 1. 只能只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法，
> 2. 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream

> 这里是为了后端可以接受数组