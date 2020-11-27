## 线程池

**Executor框架**：主线程创建实现了**Runnable**或者**Callable**接口的任务对象；将创建完成的**Runnable/Callable**接口的对象交给**ExecutorService**执行；**ExecutorService**对象会返回一个实现**Future**接口的对象。主线程可以执行**FutureTask.get()**方法来等待任务执行完成。



