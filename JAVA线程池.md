## 线程池

**Executor框架**：主线程创建实现了**Runnable**或者**Callable**接口的任务对象；将创建完成的**Runnable/Callable**接口的对象交给**ExecutorService**执行；**ExecutorService**对象会返回一个实现**Future**接口的对象。主线程可以执行**FutureTask.get()**方法来等待任务执行完成。

> 昨天晚上看了tuna的王润泽的演讲，感觉自己真的是井底之蛙，别人的世界是7000分的FS，而你的世界只是佯装姿态的太无聊了，一天天的无所事事，须知，还有好多东西你去学习的呀！

**Executor框架**：任务执行需要实现**Runnable**或者**Callable**接口。任务执行的核心接口是**Executor**接口，以及继承实现了它的**ExecutorService**接口。**ThreadPoolExecutor**和**ScheduledThreadPoolExecutor**这两个类实现了**ExecutorService**接口。需要注意的是**ScheduledThreadPoolExecutor**这个类实际上是继承了 **ThreadPoolExecutor**的。因此最需要关注的是**ThreadPoolExecutor**这个类。**Future**接口以及**Future**接口的实现类**FutureTask**类都可以代表异步计算的结果。当我们**submit(Runnable/Callable)**给一个**ThreadPoolExecutor**时，**submit**会有一个**FutureTask**的返回值。

**executor方法**：当任务提交到executor的时候，会检查一下**核心线程池**是否已满如果没有满的话，直接**addWorker(task, true)**添加一个新的核心线程执行，如果**核心线程池**满了的话，将当前任务加入到等待队列里面，如果任务的等待队列也满了，那么必须要创建非核心线程来救急，执行**addWorker(task, false)**添加一个非核心线程并执行绑定的任务。

**返回值问题**：就目前看的来说，是**Runnable**和**execute**一块儿使用，**Callable**和**submit**一块儿使用。