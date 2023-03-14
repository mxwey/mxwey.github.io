---
title: "Accurate Timer" # Title of the blog post.
date: 2023-03-13T12:32:36+08:00 # Date of post creation.
description: "" # Description used for search engine.
summary: ""
featured: false # Sets if post is a featured post, making appear on the home page side bar.
draft: true # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: false # Set to true to group assets like images in the same folder as this post.
# featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
# featureImageAlt: 'Description of image' # Alternative text for featured image.
# featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "/images/cocos.png" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 20 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: true # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - cocos
tags:
  - cocos优化

# comment: false # Disable comment if false.
---

>TimerManager.ts
```ts
/**
 * 更加精确的计时器
 */

import { Component } from "cc";

const guid = function () {

    let guid: string = "";

    for (let i = 1; i <= 32; i++) {

        let n = Math.floor(Math.random() * 16.0).toString(16);

        guid += n;

        if ((i == 8) || (i == 12) || (i == 16) || (i == 20))

            guid += "-";

    }

    return guid;

}

export class TimerManager {

    private static times: any = {};

    private schedules: any = {};

    private _scheduleCount: number = 1;

    private initTime: number = (new Date()).getTime();      // 当前游戏进入的时间毫秒值

    private component: Component;

    // 服务器时间与本地时间间隔

    private _$serverTimeElasped: number = 0;

    constructor(component: Component) {

        // super();

        this.component = component;

        this.schedule(this.onUpdate, 1/60);

    }

    /**

     * 设置服务器时间与本地时间间隔

     * @param val

     */

    public serverTimeElasped(val?: number): number {

        if (val) {

            this._$serverTimeElasped = val;

        }

        return this._$serverTimeElasped;

    }

    /**

     * 格式化日期显示 format= "yyyy-MM-dd hh:mm:ss";

     * @param format

     * @param date

     */

    public format(format: string, date: Date): string {

        let o: any = {

            "M+": date.getMonth() + 1,                      // month

            "d+": date.getDate(),                           // day

            "h+": date.getHours(),                          // hour

            "m+": date.getMinutes(),                        // minute

            "s+": date.getSeconds(),                        // second

            "q+": Math.floor((date.getMonth() + 3) / 3),    // quarter

            "S": date.getMilliseconds()                     // millisecond

        }

        if (/(y+)/.test(format)) {

            format = format.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));

        }

        for (let k in o) {

            if (new RegExp("(" + k + ")").test(format)) {

                format = format.replace(RegExp.$1, RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length));

            }

        }

        return format;

    }

    /** 获取游戏开始到现在逝去的时间 */

    public getTime(): number {

        return this.getLocalTime() - this.initTime;

    }

    /** 获取本地时间刻度 */

    public getLocalTime(): number {

        return Date.now();

    }

    public schedule(callback: Function, interval: number): string {

        let UUID = `schedule_${this._scheduleCount++}`

        this.schedules[UUID] = callback;

        this.component.schedule(callback, interval);

        return UUID;

    }

    public scheduleOnce(callback: Function, delay: number = 0): string {

        let UUID = `scheduleOnce_${this._scheduleCount++}`;

        this.schedules[UUID] = callback;

        this.component.scheduleOnce(() => {

            let cb = this.schedules[UUID];

            if (cb) {

                cb();

            }

            this.unschedule(UUID);

        }, Math.max(delay, 0));

        return UUID;

    }

    public unschedule(uuid: string) {

        let cb = this.schedules[uuid];

        if (cb) {

            this.component.unschedule(cb);

            delete this.schedules[uuid];

        }

    }

    public unscheduleAllCallbacks() {

        for (let k in this.schedules) {

            this.component.unschedule(this.schedules[k]);

        }

        this.schedules = {};

    }

    onUpdate(dt: number) {

        // 后台管理倒计时完成事件

        for (let key in TimerManager.times) {

            let data = TimerManager.times[key];

            data.update(dt);

        }

    }

    /** 游戏最小划时记录时间数据 */

    public save() {

        for (let key in TimerManager.times) {

            TimerManager.times[key].recordTime = this.getTime();

        }

    }

    /** 游戏最大化时回复时间数据 */

    public load() {

        for (let key in TimerManager.times) {

            let data:Timer = TimerManager.times[key];

            if(data.isSaveTime){

                // 经过了多少时间，单位为秒

                let interval = ((this.getTime() - (data.recordTime || this.getTime())) / 1000);

                let overTimes = interval/data.step;

                data.curTimes = data.curTimes + overTimes;

                if (data.curTimes > data.totalTime) {

                    if (data.onDelayCompleteCallback) {

                        data.onDelayCompleteCallback.call(data.object);                     // 触发超时回调事件  

                    }

                }

            }

           

            data.recordTime = 0;

        }

    }

    public getTimer(id:string):Timer{

        if (TimerManager.times[id])

            return TimerManager.times[id];

        return null;

    }

    /**

     * 注册指定对象的倒计时属性更新

     * @param object 回调对象

     * @param step 时间间隔

     * @param totalTime 执行次数（n+1次）

     * @param onStepCallback 执行回调

     * @param onDelayCompleteCallback 超时回调

     * @param isSaveTime 是否保存切后台时间

     * @returns

     */

    public addTimer(object: any, step:number,totalTime: number = -1, onStepCallback: Function, onDelayCompleteCallback?:Function,isSaveTime:boolean = false) {

        let data: Timer = new Timer(step,totalTime,onStepCallback,object,isSaveTime);

        data.id = guid();

        data.onDelayCompleteCallback = onDelayCompleteCallback; // 超时完成事件

        TimerManager.times[data.id] = data;

        return data.id;

    }

    /** 注消指定对象的倒计时属性更新 */

    public clearTimer(id: string) {

        if (TimerManager.times[id])

            delete TimerManager.times[id];

    }

}

/** 定时跳动组件 */

export class Timer {

    public id:string

    public onStepCallback: Function | null = null;

    public onDelayCompleteCallback: Function | null = null;

    public object: any | null = null;

    public totalTime:number = -1;//总次数，-1为无限

    public recordTime:number = 0;//某个时间点保存的时间

    public curTimes:number = 0;//当期触发次数

    public isSaveTime:boolean = false;//是否记录切后台时间

    private _elapsedTime: number = 0;

    private _isStop:boolean = false;

   

    public get elapsedTime(): number {

        return this._elapsedTime;

    }

    private _step: number = 0;

    /** 触发间隔时间（秒） */

    get step(): number {

        return this._step;

    }

    set step(step: number) {

        this._step = step;                     // 每次修改时间

        this._elapsedTime = 0;                 // 逝去时间

    }

    public get progress(): number {

        return this._elapsedTime / this._step;

    }

    /**

     *

     * @param step 时间间隔

     * @param totalTimes 总次数，-1为无限

     * @param callback 回调

     * @param object 回调者

     */

    constructor(step: number = 0,totalTimes:number = -1,callback?:Function,object?:any,isSaveTime?:boolean) {

        this.step = step;

        this.onStepCallback = callback;

        this.object = object;

        this.totalTime = totalTimes;

        this.curTimes = 0;

        this.isSaveTime = isSaveTime;

        this.onStepCallback?.call(this.object,this.curTimes,(this.totalTime - this.curTimes));

    }

    public update(dt: number) {

        if (this._isStop){

            return;

        }

        this._elapsedTime += dt;

        if (this._elapsedTime >= this._step) {

            this._elapsedTime -= this._step;

            if(this.totalTime == -1){

                this.onStepCallback?.call(this.object);

                return true;

            }else{

               

                if (this.curTimes < this.totalTime){

                    this.curTimes++;

                    this.onStepCallback?.call(this.object,this.curTimes,(this.totalTime - this.curTimes));

                    return true;

                }

            }

            return false;

           

        }

        return false;

    }

    // 重置定时器

    public reset(step?:number,totalTimes?:number) {

        this._elapsedTime = 0;

        this.curTimes = 0;

        if (step){

            this.step = step;

        }

        if (totalTimes){

            this.totalTime = totalTimes;

        }

    }

    public isStop(){

        return this._isStop;

    }

    // 停止

    public stop(){

        this._isStop = true;

        this.reset();

    }

    // 开始

    public start(){

        this._isStop = false;

        this.reset();

    }

    // 暂停

    public pause(){

        this._isStop = true;

    }

    // 恢复

    public resume(){

        this._isStop = false;

    }

}
```


> 实例代码
```ts

import { _decorator, Component, game, js, Game } from 'cc';

import { Timer, TimerManager } from './TimerManager';

const { ccclass, property } = _decorator;
 

@ccclass('Main')

export class Main extends Component {

    private _timerId = null;

    private timerManager:TimerManager;

    start () {

        let date = new Date().getTime();

        console.log(this.getDateStr(date));

        this.timerManager = new TimerManager(this);

        // this._timerId = this.timerManager.addTimer(this,0.025,1000,this.onTimerHandler);

        this._timerId = this.timerManager.addTimer(this,1,10,this.onTimerHandler,this.onDelayTimerHandler,true);

        // 游戏显示（进入前台）

        game.on(Game.EVENT_SHOW, () => {

            this.timerManager.load();

        });

   

        // 游戏隐藏事件（进入后台）

        game.on(Game.EVENT_HIDE, () => {

            this.timerManager.save();

        });

    }

    click(){

        let timer = this.timerManager.getTimer(this._timerId);

        if (timer.isStop()){

            // 恢复

            timer.resume();

        }else{

            // 暂停

            timer.pause();

        }

    }

    /**

     *

     * @param curTimes 已经过时间

     * @param remainTimes 剩下的时间

     */

     onTimerHandler(curTimes,remainTimes){

        console.log("==onTimerHandler:",curTimes,remainTimes);

        if(remainTimes == 0){

            let date = new Date().getTime();

            console.log(this.getDateStr(date));

        }

    }

    /**超时执行 */

    onDelayTimerHandler(){

        console.log("==onDelayTimerHandler");

    }

    getDateStr(time:any) {

        var date = new Date(time);

        var y = date.getFullYear();

        var m = date.getMonth() + 1;

        var d = date.getDate();

        var h = date.getHours();

        var mm = date.getMinutes();

        var s = date.getSeconds();

        return js.formatStr("%s-%s-%s %s:%s:%s", y, m, d, h, mm, s);

    }

}
```