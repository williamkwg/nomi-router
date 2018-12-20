## Installation

```bash
$ npm install nomi-router --save
```

Node.js >= 8.0.0  required.

## API

### property

- notation.RequestMapping
- notation.Service

### function

- init

## Usage

#### demo1: the usage of RequestMapping and Service in Controller.

``` javascript

const { RequestMapping, Service }  = require('nomi-router').notation;
 
@RequestMapping({
    path:"/order/{userId:num}",
    method:"post",
    middleware:['md1',"md1"]
})
class OrderController{
 
    @Service('service.orderService')
    serviceInst;
 
    @Service
    orderService;
 
    @RequestMapping({path:"/all/{type:string}",method:"get"})
    async index(req,res,paras,ctx) {
        res.body = this.serviceInst.loadAllOrders();
    }
 
    @RequestMapping("/user/{num:num}")
    async user(req,res,paras,ctx) {
        res.body = this.orderService.loadUserOrders();
    }
}
exports.OrderController = OrderController;
 ```

#### demo2: the usage of Service in Service.

``` javascript
const { Service }  = require('nomi-router').notation;
 
@Service('user.orderService')
class OrderService{
    async loadAllOrders() {
        console.log("loadAllOrders");
    }
 
    loadUserOrders(){
        console.log("loadUserOrders");
    }
}
exports.OrderService = OrderService;
```

#### demo3: the usage of init api

``` javascript

const r = require("nomi-router");
const router = await r.init({
        controllerPath:["/Controller/users","/Controller/orders"],
        servicePath:["/Service/users","/Service/orders"]
    });

/**
 * actObj: {
 *    action: {
 *        path: '/user/{userId: 123}/type/{type: string}',
 *        method: 'post',
 *        middleware: ['md1', 'md2'],
 *        action: [AsyncFunction: index],
 *        controller: OrderController { serviceInst: OrderService, orderService: OrderService },
 *        act: [Function: act]
 *    },
 *    paras: {
 *        userId: 123,
 *        type: 'hotel'
 *    }
 * }
*/
const actObj = router.match('/user/123/type/badorder','post');
```
