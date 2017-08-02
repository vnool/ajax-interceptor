/*
 * @Author: DingChengliang
 * @Date:   2017-04-13 16:42:30
 * @Last Modified by:   DingChengliang
 * @Last Modified time: 2017-04-25 16:29:34
 * @Description: ajax Interceptor
 */
'use strict';

class AjaxInterceptor {
    constructor(list = []) {
        this.init(list);
    }
    init(interceptorList) {
        this.interceptorList = interceptorList;

        this.replaceAjax(interceptorList);

    }

    /**
    handle HTTP request and response
    **/
    responseProcess(url, post, response, xhr) {

        for (var index in this.interceptorList) {
            var itpt = this.interceptorList[index]; //所有挂载的函数
            var type = itpt.type;
            if (url.indexOf(itpt.keyword) > 0) {
                var returnJson = (itpt.dataType == 'json' || typeof(itpt.dataType) == 'undefined') ? true : false;
                if (typeof(itpt.request) != 'undefined') {
                    if (returnJson) {
                        itpt.request({
                            'post': post && JSON.parse(post),
                            'response': response && JSON.parse(response),
                        });
                    } else {
                        itpt.request({
                            'post': post,
                            'response': response,
                        });
                    }
                }
                if (typeof(itpt.response) != 'undefined') {
                    if (returnJson) {
                        itpt.response(response && JSON.parse(response));
                    } else {
                        itpt.response(response);
                    }
                }

                if (itpt.cancle) {
                    return;
                }

            } //matched
        }
    }


    //replace the XMLHttpRequest
    replaceAjax() { // Overriding XMLHttpRequest
        var This = this;
        (function() {
            var oldXHR = window.XMLHttpRequest;

            function newXHR() {
                var realXHR = new oldXHR();
                var oldSend = realXHR.send;
                realXHR.send = function(postBody) {
                    this.postBody = postBody;
                    return oldSend.apply(this, arguments);
                }
                realXHR.addEventListener("readystatechange", function(event) {
                    try {
                        var tg = event.currentTarget;
                        if (tg.readyState != this.DONE) {
                            return;
                        }
                        This.responseProcess(tg.responseURL, tg.postBody, tg.responseText, tg);
                    } catch (e) {
                        console.log(e)
                    }
                }, false);
                return realXHR;
            }
            window.XMLHttpRequest = newXHR;
        })(); //run
    }
}
module.exports = AjaxInterceptor;
