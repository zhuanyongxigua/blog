---
title: 练习用原生JavaScript编写与JavaScript数组方法slice功能一样的函数
date: 2017-06-28 21:01:40
---
刚刚开始的时候感觉不太难，越写越发现之前忽略的可能性很多。

当只有一个变量的时候，把代码写出来不是特别困难。可当有两个变量的时候，情况就复杂了很多。我在刚开始写的时候就在想如何把冗余的代码裁减掉，结果就是大脑宕机。最后老老实实的在纸上，用最笨的方法把所有的可能性都列出来，再去想如何把类似的代码拿出来就变得很容易了。

所以，如果觉得大脑硬件不够用了，那就用最笨的办法先去把事情做了，然后再去优化。

最后写出来的东西

~~~
Array.prototype.mySlice = function () {
    var n = arguments[0];
    var m = arguments[1];
    if (n >= 0) {
        var nN = Math.floor(n);
    }else {
        var nN = Math.ceil(n);
    }
    if (m >= 0) {
        var nM = Math.floor(m);
    }else {
        var nM = Math.ceil(m);
    }
    var aArray1 = [];
    if (arguments.length == 1) {
        if (nN <= -this.length) {
            for (var i = 0; i < this.length; i++) {
                aArray1[i] = this[i];
            }
            return aArray1;
        }
        else if (nN > -this.length && nN < 0) {
            for (var i = 0; i < -nN; i++) {
                aArray1[i] = this[nN + this.length + i];
            }
            return aArray1;
        }else if (nN == 0) {
            for (var i = 0; i < this.length; i++) {
                aArray1[i] = this[i];
            }
            return aArray1;
        }else if (nN > 0 && nN <= this.length) {
            for (var i = 0; i < this.length - nN; i++) {
                aArray1[i] = this[nN + i]
            }
            return aArray1;
        }
        else {
            return [];
        }
    } else {
        if (nN + 1 > this.length) {
            return [];
        }else if (nM < -this.length) {
            return [];
        }else {
            if (nN >= 0) {
                if (nM >= 0 && nM <= this.length-1) {
                    if (nN >= nM) {
                        return [];
                    }else {
                        for (var i = 0; i < (nM - nN); i++) {
                            aArray1[i] = this[nN + i];
                        }
                        return aArray1;
                    }
                }
                else if (nM > this.length-1) {
                    for (var i = 0; i < this.length - nN; i++) {
                        aArray1[i] = this[nN + i];
                    }
                    return aArray1;
                }
                else if (nM <= -1) {
                    if ((-nM + nN) >= this.length) {
                        return [];
                    }else {
                        for (var i = 0; i < (this.length + nM - nN); i++) {
                            aArray1[i] = this[nN + i];
                        }
                        return aArray1;
                    }
                }
                else {
                    return [];
                }

            }
            else if (nN <= -1 && nN > -this.length) {
                if (nM > 0 && nM <= this.length-1) {
                    if (- nN + nM <= this.length) {
                        return [];
                    }else {
                        for (var i = 0; i < nM - nN - this.length; i++) {
                            aArray1[i] = this[nN + this.length + i];
                        }
                        return aArray1;
                    }
                }
                else if (nM > this.length-1) {
                    for (var i = 0; i < -nN; i++) {
                        aArray1[i] = this[this.length + nN + i];
                    }
                    return aArray1;
                }
                else if (nM <= -1) {
                    if (nN < nM) {
                        for (var i = 0; i < (nM - nN); i++) {
                            aArray1[i] = this[this.length + nN + i];
                        }
                        return aArray1;
                    }else {
                        return [];
                    }
                }
                else {
                    return [];
                }
            }
            else {
                if (nM > 0 && nM <= this.length -1) {
                    for (var i = 0; i < nM; i++) {
                        aArray1[i] = this[i];
                    }
                    return aArray1;
                }else if (nM <= -1 && nM > -this.length) {
                    for (var i = 0; i < (this.length + nM); i++) {
                        aArray1[i] = this[i];
                    }
                    return aArray1;
                }else if (nM > this.length-1) {
                    for (var i = 0; i < this.length; i++) {
                        aArray1[i] = this[i]
                    }
                    return aArray1;
                }
                else {
                    return [];
                }
            }
        }
    }
}
//测试部分。
var aTest = [0, 1, 2, 3, 4];
var b = [];
for (var i = 0; i < 10; i++) {
    var n = Math.random() * 100 - 50;
    b[i] = n;
}
for (var i = 0; i < b.length; i++) {
    for (var j = 0; j <b.length; j++) {
        console.log(b[i], b[j]);
        console.log(aTest.slice(b[i], b[j]));
        console.log(aTest.mySlice(b[i], b[j]));
    }
}
//测试一个参数的情况。
// for (var i = 0; i < a.length; i++) {
//         console.log(b[i]);
//         console.log(aTest.slice(b[i]));
//         console.log(aTest.mySlice(b[i]));
// }
~~~
