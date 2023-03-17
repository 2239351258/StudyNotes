### 观察者模式

```ts
// 观察者
interface IObserver {
  // 观察者被通知后执行的方法
  update(params: any);
}
// 被观察主题
interface ISubject {
  // 添加观察者
  registerObserver(observer: Observer);
  // 移除观察者
  removeObserver(observer: Observer);
  // 通知观察者
  notifyObserver();
}

class Observer implements IObserver {
  update(state: number) {
    console.log(`数据已更新为：${state}`);
  }
}

class Subject implements ISubject {
  /**观察者列表 */
  private observers: Observer[] = [];
  private _state: number;
  /**state是否变化 */
  set state(state: number) {
    this._state = state;
    this.notifyObserver();
  }
  get state() {
    return this._state;
  }
  /**
   * 注册观察者
   * @param observer 观察者
   */
  registerObserver(observer: Observer): void {
    //判断该观察者是否已注册
    if (!this.observers.some(item => item === observer )) {
      this.observers.push(observer);
    }
  }
  /**移除观察者 */
  removeObserver(observer: Observer) {
    if (this.observers.length > 0) {
      this.observers = this.observers.filter(item => item !== observer);
    }
  }
  /**通知所有观察者 */
  notifyObserver() {
    this.observers.forEach(item => {
      item.update(this._state);
    })
  }
}
const sub = new Subject()
const obs1 = new Observer()
const obs2 = new Observer()

sub.registerObserver(obs1)
sub.registerObserver(obs2)
sub.state = 1
```

### 发布订阅者模式

```ts
interface IEvent {
    on: (name: string, fn: Function) => void
    emit: (name: string, ...args: Array<any>) => void
    off: (name: string, fn?: Function) => boolean
    once: (name: string, fn: Function) => void
}

interface IArr {
    [key: string]: Array<Function>
}
class Events implements IEvent{
    /**事件对象 */
    eventObj:IArr = {};
    constructor() { }
    /**
     * 发布(触发订阅的事件)
     * @param name 要发布的主题
     * @param arg 作为事件发送的数据
     */
    emit(name: string, ...args: any[]):void {
        if (this.eventObj.hasOwnProperty(name)) {
            const fn = this.eventObj[name];
            fn.forEach(fn => {
                fn.apply(this, args);
            });
        }
    }
    /**
     * 订阅
     * @param name 要订阅的主题
     * @param fn 事件处理函数
     */
    on(name: string, ...fn: Function[]): void {
        if (!this.eventObj.hasOwnProperty(name)) {
            this.eventObj[name] = [];
        }
        this.eventObj[name].push(...fn);
    }
    /**
     * 取消订阅
     * @param name 订阅的主题
     * @param fn 要取消的事件处理函数
     */
    off(name: string, fn?: Function): boolean {
        if (!this.eventObj.hasOwnProperty(name)) {
            return false;
        }
        if (fn) {
            // 如果处理函数存在，则从主题处理数组中删除它
            this.eventObj[name] = this.eventObj[name].filter(x => x !== fn);
            return true;
        } else {
            // 否则直接删除该主题
            return delete this.eventObj[name];
        }
    }
    once(name: string, fn: Function): void{
        let fake = (...args: Array<any>) => {
            fn.apply(this, args)
            this.off(name, fake)
        }
        this.on(name, fake)
    }
}

const d = new Events()

d.on('ys', (...args: Array<any>) => {
console.log('on', args)
})
d.once('ys', (...args: Array<any>) => {
console.log('once', args)
})

const fn = (...args: Array<any>) => {
console.log('fn', args)
}
d.on('ys', fn)
d.emit('ys', '亚索', { name: '亚索', age: 12 })
d.emit('ys', '瑞雯', { name: '瑞雯', age: 20 })
d.off('ys', fn)
d.emit('ys', '鳄鱼', { name: '鳄鱼', age: 108 })
```

