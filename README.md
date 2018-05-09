# SaltedFish
## 基于区块链游戏--无聊的咸鱼 [体验地址](http://www.nanayun.cn/saltedFish.html)
![](http://cdn.binghai.site/o_1cd2kittd1lud1kmo3sdgjekdta.png)
### 在座的各位都很无聊，不如来比一比谁更无聊吧
### 合约内容
```
'use strict';

var SaltedFish = function (text) {
  if (text) {
    var o = JSON.parse(text);
    this.num = o.num;
    this.from = o.from;
  }
};


SaltedFish.prototype = {
  toString: function () {
    return JSON.stringify(this);
  }
};

var BoringBoard = function () {
  LocalContractStorage.defineProperty(this, "fish");
  LocalContractStorage.defineMapProperty(this, "boringBoard", {
    parse: function (text) {
      return new SaltedFish(text);
    },
    stringify: function (o) {
      return o.toString();
    }
  });
};

BoringBoard.prototype = {
  init: function () {this.fish = 0;},

  iAmSaltedFish:function(){
    var from = Blockchain.transaction.from;
    
    for(var i = 0; i < this.fish;i++){
        var sf = this.getFish(i);
        if(sf.from == from){
            sf.num += 1;
            this.boringBoard.put(i,sf);
            return sf;
        }
    }

    var sf = new SaltedFish();
    sf.num = 1;
    sf.from = from;
    this.boringBoard.put(this.fish,sf);
    this.fish += 1;
    return sf;
  },

  getFish:function(index){
    return this.boringBoard.get(index);
  },

  fishList:function(){
    var list = [];
    for(var i = 0; i < this.fish;i++){
        var sf = this.getFish(i);
        list.push(sf);
    }
    return list;
  },
};
module.exports = BoringBoard;
```
