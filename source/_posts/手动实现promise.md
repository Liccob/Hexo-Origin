---
title: 手动实现promise
date: 2018-09-06 10:29:39
tags:
---

看网上教程手动实现

```
function Promise(executor){

	var self = this;
	self.status = 'pending';
	self.data = undefined;
	self.onResolvedCallback = [];
	self.onRejectCallback = [];

	function resolve(data){
		if(self.status === 'pending'){
			self.status = 'resolved';
			self.data = data;
			for(var i = 0 ; i<self.onResolvedCallback.length;i++){
				self.onResolvedCallback[i](data)
			}
		}
	};
	function reject(reason){
		if (self.status === 'pending') {
      self.status = 'rejected'
      self.data = reason
      for(var i = 0; i < self.onRejectedCallback.length; i++) {
        self.onRejectedCallback[i](reason)
      }
    }
	};

	try {
		executor(resolve,reject);
	} catch (error) {
		reject(error);
	}

}

Promise.prototype.then = function(onResolve,onReject){
	var self = this;
	var Promise2;

	onResolve = typeof onResolve === 'function'? onResolve : function(v){ return v};
	onReject  = typeof onReject  === 'function'? onReject  : function(r){ return r};
	if (self.status === 'resolved') {
    return promise2 = new Promise(function(resolve, reject) {
			try {
				var x = onResolve(self.data);
				if(x instanceof Promise){
					x.then(resolve,reject);
				}
				resolve(x)
			} catch (error) {
				reject(error)
			}
    })
  }

  if (self.status === 'rejected') {
    return promise2 = new Promise(function(resolve, reject) {
			try {
        var x = onRejected(self.data)
        if (x instanceof Promise) {
          x.then(resolve, reject)
        }
      } catch (e) {
        reject(e)
      }
    })
  }

  if (self.status === 'pending') {
    return promise2 = new Promise(function(resolve, reject) {
			self.onResolvedCallback.push(function(value) {
        try {
          var x = onResolved(self.data)
          if (x instanceof Promise) {
            x.then(resolve, reject)
          }
        } catch (e) {
          reject(e)
        }
      })

      self.onRejectedCallback.push(function(reason) {
        try {
          var x = onRejected(self.data)
          if (x instanceof Promise) {
            x.then(resolve, reject)
          }
        } catch (e) {
          reject(e)
        }
      })
    })
  }

}

```