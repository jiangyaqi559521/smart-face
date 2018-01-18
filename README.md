/*
 * created by deepdot on 2017/03/28
 */
// 定义全局定时器变量
var bigEarthInterval = null;
var shibieInterval = null;
var outpointInterval = null;
var circleInterval = null;
var circleInterval = null;
var videoShow = false;

$(function () {
    var ws_path = "ws://"+ window.location.host +"/wsnewtrack";
    //console.log("Connecting to " + ws_path)
    var webSocketBridge = new channels.WebSocketBridge();
    webSocketBridge.connect(ws_path);
    webSocketBridge.listen(function (data) {
        var facetrack_id = data["facetrack_info"]["facetrack_id"];
        var facetrack_img_fn = data["facetrack_info"]["imgs"][0]["fn"];
        var person_id = data["person_info"]["person_id"];
        var score = data["score"];
        var status = data["status"];
        var status_info=data["status_info"];
        var track_img = "<img src=/image?"
            + "fn=" + facetrack_img_fn
            + "&id=" + facetrack_id
            + "&type=4"
            +">";
        var person_img = "<img src=/image?"
                + "fn="
                + "&id=" + person_id
                + "&type=5"
                +">";
        if(data["status"]==1){
            var person_name = data["person_info"]["name"];
            var person_content = "<h4 id='p3'>"
                + person_name
                + "</h4>";
            $("#img2").empty();
            $("#img2").prepend(person_img);
            $(".content").prepend(person_content);
            staffPerson();
        }
        else if(data["status"]==-1) {
            $("#img2").empty();
            strangerPerson();
            $("#img2").prepend(track_img);
        }else if(data["status"]==-2) {
            $("#img2").empty();
            createNormalPerson()
            $("#img2").prepend(track_img);
        }else if(data["status"]==-3) {
            $("#img2").empty();
            Err();
            $("#img2").prepend(track_img);
        }else if(data["status"]==-3) {
            $("#img2").empty();
            backImgAnimate();
            $("#img2").prepend(track_img);
        }else if(data["status"]==2){
            $("#img2").empty();
            $("#img2").prepend(person_img);
            shuacard();

        }
    });
    webSocketBridge.socket.onopen = function() { console.log("Connected to notification socket"); }
    webSocketBridge.socket.onclose = function() { console.log("Disconnected to notification socket"); }
});


function startReader() {
	if(videoShow){
		return
	}
	videoShow = true;
    clearInterval(outpointInterval);
    clearInterval(circleInterval);
    clearInterval(bigEarthInterval);
    //判断状态是否启动
    //实时画面弹出
    //$('#backgroundImg').css('z-index', 0);
//  $('header .videoboxImg').css({'opacity': '1', "transition": "all 0.3s"});
    //改变时间和日期字体
   // $('#midCircle .midtimeContent p#currentTime').animate({ 'font-size': '66px' },200,'linear');
    //$('#midCircle .midtimeContent p#currentDay').animate({ 'font-size': '18px' },200,'linear');
//  $('.videobox').css({
//  		'margin': '0 auto',
//  		'transition': 'all 0.3s'
//  });
	//$('.videobox').css({
    //		'width': '57.5%',
    //		'height': '100%',
    //		'margin': '0 auto',
    //		'transition': 'all 0.3s'
   // });

	//	缩小 中心球形div
	//$('#midCircle .midtime>div').animate({ 'width': '350px', 'height': '350px' }, 150, function() {
	//	$('#midCircle .midtime>div canvas').css('opacity', 1);
	//	$('#midCircle .out').css( 'opacity', 1 ); //外侧光点 环形
	//});
	//	内圈环形

}

//背景图片收缩
var BIA = false;
function backImgAnimate() {
	if(BIA){
		return
	}
	BIA = true;
    clearInterval(shibieInterval);
    var j = 0;
    var ctx = document.getElementById('shibie');
    return shibieInterval = self.setInterval(function() {
        var shibieimg = new Image();
        shibieimg.src = static_dir + "images/shibie/shibie_" + j + ".png";
        shibieimg.onload = function() {
            if (j < 27) {
                ctx.getContext('2d').clearRect(0, 0, ctx.width, ctx.height);
                ctx.getContext('2d').drawImage(shibieimg, 0, 0, ctx.width, ctx.height);
                j++;
            } else {
                ctx.getContext('2d').clearRect(0, 0, ctx.width, ctx.height);
                ctx.getContext('2d').drawImage(shibieimg, 0, 0, ctx.width, ctx.height);
                j = 27;
                BIA = false;
            }
        }

    }, 60)

}

//特殊用户展示
function specialPerson(imgUrl) {
    //取消粒子动画
    clearInterval(outpointInterval);
    clearInterval(circleInterval);
    clearTimeout(timeout);
    $("#p3").remove();
    var obj = document.getElementById("p");
    var obj2 = document.getElementById("p2");
    obj2.innerText= "生日快乐!";
    obj.innerText= " ";
    $('#touxiang').show();
    $('.content').css("margin-top","0");
    $('#img2').hide();
    $("#img1").show();
    //中心环形动画消失
    $('.out').animate({ 'opacity': 0 }, 100, function() {
		$('.outpointbox').animate({ 'opacity': 1 }, 100)
    });
        //中心球形消失
    $('#centerCircle').animate({ 'opacity': 0 }, 100, function() {
        //时间窗口上移
        $('.midtimeContent').animate({ 'top': '-115px' },100);
        $('#specialbox').animate({ 'opacity': 1 }, 100);
    });
    var touxiang = imgUrl;
    var cas = document.getElementById('touxiang');
    var touxiang = new Image();
    touxiang.src = imgUrl;
    cas.getContext('2d').clearRect(0, 0, cas.width, cas.height);
    cas.getContext('2d').drawImage(touxiang, 0, 0, cas.width, cas.height)

    var i = 101;
    var cvs = document.getElementById('specialCanvas');
    circleInterval = self.setInterval(function() {
		var img = new Image();
		img.src = static_dir + 'images/special/special' + i + '.png';
		img.onload = function() {
			if (i < 301) {
				cvs.getContext('2d').clearRect(0, 0, cvs.width, cvs.height);
				cvs.getContext('2d').drawImage(img, 0, 0, cvs.width, cvs.height);
				i++;
			} else {
				cvs.getContext('2d').clearRect(0, 0, cvs.width, cvs.height);
				cvs.getContext('2d').drawImage(img, 0, 0, cvs.width, cvs.height);
				i = 101;
			}
		}
	}, 60)
    //	规定秒数后 返回激活状态
    var timeout = setTimeout(function() {
        $('#specialbox').css({  //特殊弹窗消失
        		'opacity': 0,
        		'transition': 'all 0.1s'
        });
        clearInterval(circleInterval); //清除循环
        $('.out').css({
        		'opacity': 1,
        		'transition': 'all 0.1s'
        })
        setTimeout(function(){
        		$('.midtimeContent').css({
        			'top': '50%',
        			'opacity': 1,
        			'transition': 'all 0.1s'
        		});
        		$('#centerCircle').css({
        			'opacity': 1,
        			'transition': 'all 0.1s'
        		});

        		outCircle();
        },300)

    }, 2000)// 特殊用户框持续时间
}

/*中心圆外圈动画*/
function outCircle() {
    clearInterval(outpointInterval);
    var outpointcvs = document.getElementById('outpoint');
    var i = 101;
    return outpointInterval = setInterval(function() {
        var outpointImg = new Image();
        outpointImg.src = static_dir + 'images/outpoint/outpoint' + i + '.png';
        outpointImg.onload = function() {
            if (i <= 981) {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height); //清除画布
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i+=2;
            } else {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height);
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i = 101;
            }
        }
    }, 60)
}
/*中心圆内圈动画*/



function bigEarth() {
    clearInterval(bigEarthInterval);
    $('#currentTime').css('font-size', '77px');
    $('#currentDay').css('font-size', '21px');
    var j = 0;
    var cts = document.getElementById('centerCircle');
    return bigEarthInterval = self.setInterval(function() {
        var bigEarthimg = new Image();
        bigEarthimg.src = static_dir + "images/100/all_000" + j + ".png";
        bigEarthimg.onload = function() {
            if (j < 68) {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j++;
            } else {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j = 0;
            }
        }

    }, 40)
}





function strangerPerson() {
    $("#p3").remove();
    var obj = document.getElementById("p");
    var obj2 = document.getElementById("p2");
    obj2.innerText= "亲爱的陌生人";
    obj.innerText= "欢迎来到云识图！";
    $('#touxiang').hide();
    $('#img1').hide();
    $("#img2").attr("src", "images/andy.jpg").show();
    $('.content').css("margin-top","35px");

    //中心环形动画消失
    $('.out').animate({ 'opacity': 0 }, 100, function() {
        $('.outpointbox').animate({ 'opacity': 1 }, 100)
    });
    //中心球形消失
    $('#centerCircle').animate({ 'opacity': 0 }, 100, function() {
        //时间窗口上移
        $('.midtimeContent').animate({ 'top': '-115px' }, 100);
        $('#specialbox').animate({ 'opacity': 1 }, 100);
    });



    //	规定秒数后 返回激活状态
    var timeout = setTimeout(function() {
        $('#specialbox').css({  //特殊弹窗消失
            'opacity': 0,
            'transition': 'all 0.1s'
        });
        clearInterval(circleInterval); //清除循环
        $('.out').css({
            'opacity': 1,
            'transition': 'all 0.1s'
        })
        setTimeout(function(){
            $('.midtimeContent').css({
                'top': '50%',
                'opacity': 1,
                'transition': 'all 0.1s'
            });
            $('#centerCircle').css({
                'opacity': 1,
                'transition': 'all 0.1s'
            });

            outCircle();
        },300)

    }, 2000)// 特殊用户框持续时间
}

/*中心圆外圈动画*/
function outCircle() {
    clearInterval(outpointInterval);
    var outpointcvs = document.getElementById('outpoint');
    var i = 101;
    return outpointInterval = setInterval(function() {
        var outpointImg = new Image();
        outpointImg.src = static_dir + 'images/outpoint/outpoint' + i + '.png';
        outpointImg.onload = function() {
            if (i <= 981) {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height); //清除画布
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i+=2;
            } else {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height);
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i = 101;
            }
        }
    }, 60)
}
/*中心圆内圈动画*/



function bigEarth() {
    clearInterval(bigEarthInterval);
    $('#currentTime').css('font-size', '77px');
    $('#currentDay').css('font-size', '21px');
    var j = 0;
    var cts = document.getElementById('centerCircle');
    return bigEarthInterval = self.setInterval(function() {
        var bigEarthimg = new Image();
        bigEarthimg.src = static_dir + "images/100/all_000" + j + ".png";
        bigEarthimg.onload = function() {
            if (j < 68) {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j++;
            } else {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j = 0;
            }
        }

    }, 40)

}



function staffPerson() {
    var obj = document.getElementById("p");
    var obj2 = document.getElementById("p2");
    obj2.innerText= "亲爱的小伙伴";
    obj.innerText= "开始你的工作吧！";
    $('#img1').hide();
    $('#touxiang').hide();
    $("#img2").attr("src", "images/andy.jpg").show();
    $('.content').css("margin-top","35px");


    //中心环形动画消失
    $('.out').animate({ 'opacity': 0 }, 100, function() {
        $('.outpointbox').animate({ 'opacity': 1 }, 200)
    });
    //中心球形消失
    $('#centerCircle').animate({ 'opacity': 0 }, 100, function() {
        //时间窗口上移
        $('.midtimeContent').animate({ 'top': '-115px' }, 100);
        $('#specialbox').animate({ 'opacity': 1 }, 100);
    });



    //	规定秒数后 返回激活状态
    var timeout = setTimeout(function() {
        $('#specialbox').css({  //特殊弹窗消失
            'opacity': 0,
            'transition': 'all 0.1s'
        });
        clearInterval(circleInterval); //清除循环
        $('.out').css({
            'opacity': 1,
            'transition': 'all 0.3s'
        })
        setTimeout(function(){
            $('.midtimeContent').css({
                'top': '50%',
                'opacity': 1,
                'transition': 'all 0.3s'
            });
            $('#centerCircle').css({
                'opacity': 1,
                'transition': 'all 0.3s'
            });

            outCircle();
        },300)

    }, 2000)// 特殊用户框持续时间
}

/*中心圆外圈动画*/
function outCircle() {
    clearInterval(outpointInterval);
    var outpointcvs = document.getElementById('outpoint');
    var i = 101;
    return outpointInterval = setInterval(function() {
        var outpointImg = new Image();
        outpointImg.src = static_dir + 'images/outpoint/outpoint' + i + '.png';
        outpointImg.onload = function() {
            if (i <= 981) {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height); //清除画布
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i+=2;
            } else {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height);
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i = 101;
            }
        }
    }, 60)
}
/*中心圆内圈动画*/
function bigEarth() {
    clearInterval(bigEarthInterval);
    $('#currentTime').css('font-size', '77px');
    $('#currentDay').css('font-size', '21px');
    var j = 0;
    var cts = document.getElementById('centerCircle');
    return bigEarthInterval = self.setInterval(function() {
        var bigEarthimg = new Image();
        bigEarthimg.src = static_dir + "images/100/all_000" + j + ".png";
        bigEarthimg.onload = function() {
            if (j < 68) {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j++;
            } else {
                cts.getContext('2d').clearRect(0, 0, cts.width, cts.height);
                cts.getContext('2d').drawImage(bigEarthimg, 0, 0, cts.width, cts.height);
                j = 0;
            }
        }

    }, 40)
}

function shuacard() {
    $("#p3").remove();
    var obj = document.getElementById("p");
    var obj2 = document.getElementById("p2");
    obj2.innerText= "刷卡成功！";
    obj.innerText= "欢迎来到云识图！";
    $('#touxiang').hide();
    $('#img1').hide();
    $("#img2").attr("src", "images/andy.jpg").show();
    $('.content').css("margin-top","35px");

    //中心环形动画消失
    $('.out').animate({ 'opacity': 0 }, 100, function() {
        $('.outpointbox').animate({ 'opacity': 1 }, 100)
    });
    //中心球形消失
    $('#centerCircle').animate({ 'opacity': 0 }, 100, function() {
        //时间窗口上移
        $('.midtimeContent').animate({ 'top': '-115px' }, 100);
        $('#specialbox').animate({ 'opacity': 1 }, 100);
    });

    //	规定秒数后 返回激活状态
    var timeout = setTimeout(function() {
        $('#specialbox').css({  //特殊弹窗消失
            'opacity': 0,
            'transition': 'all 0.1s'
        });
        clearInterval(circleInterval); //清除循环
        $('.out').css({
            'opacity': 1,
            'transition': 'all 0.1s'
        })
        setTimeout(function(){
            $('.midtimeContent').css({
                'top': '50%',
                'opacity': 1,
                'transition': 'all 0.1s'
            });
            $('#centerCircle').css({
                'opacity': 1,
                'transition': 'all 0.1s'
            });

            outCircle();
        },300)

    }, 2000)// 特殊用户框持续时间
}

/*中心圆外圈动画*/
function outCircle() {
    clearInterval(outpointInterval);
    var outpointcvs = document.getElementById('outpoint');
    var i = 101;
    return outpointInterval = setInterval(function() {
        var outpointImg = new Image();
        outpointImg.src = static_dir + 'images/outpoint/outpoint' + i + '.png';
        outpointImg.onload = function() {
            if (i <= 981) {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height); //清除画布
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i+=2;
            } else {
                outpointcvs.getContext('2d').clearRect(0, 0, outpointcvs.width, outpointcvs.height);
                outpointcvs.getContext('2d').drawImage(outpointImg, 0, 0, outpointcvs.width, outpointcvs.height);
                i = 101;
            }
        }
    }, 60)
}
/*中心圆内圈动画*/

