### Android系统架构

|          应用程序           |
| :-------------------------: |
|        应用程序框架         |
| 系统运行库（Android运行时） |
|          Linux内核          |

### Android常用组件

1. **Activity：** 是与用户交互的入口点。它表示拥有界面的单个屏幕。例如，电子邮件应用可能有一个显示新电子邮件列表的 Activity、一个用于撰写电子邮件的 Activity 以及一个用于阅读电子邮件的 Activity。尽管这些 Activity 通过协作在电子邮件应用中形成一种紧密结合的用户体验，但每个 Activity 都独立于其他 Activity 而存在。因此，其他应用可以启动其中任何一个 Activity（如果电子邮件应用允许）。例如，相机应用可以启动电子邮件应用内用于撰写新电子邮件的 Activity，以便用户共享图片。Activity 有助于完成系统和应用程序之间的以下重要交互：

   - 追踪用户当前关心的内容（屏幕上显示的内容），以确保系统继续运行托管 Activity 的进程。
   - 了解先前使用的进程包含用户可能返回的内容（已停止的 Activity），从而更优先保留这些进程。
   - 帮助应用处理终止其进程的情况，以便用户可以返回已恢复其先前状态的 Activity。
   - 提供一种途径，让应用实现彼此之间的用户流，并让系统协调这些用户流。（此处最经典的示例是共享。）

2. **Intent**：Intent并不是Android应用的组件，它是Android应用内不同组件之间通信的载体。当Android运行时需要连接不同的组件时，通常就需要借助于Intent来实现。

   　　Intent可以启动应用中的另一个Activity，也可以启动一个Service组件，还可以发送一条广播消息来触发系统中的BroadcastReceiver。也就是说，Activity、Service、BroadcastReceiver三种组件之间的通信都以Intent作为载体，只是不同组件使用Intent的机制略有区别而已。

   - Activity：当需要启动一个Activity时，可嗲用Context的startActivity(Intent intent)或startActivityForResult(Intent intent,int requestCode)方法，这两个方法中的Intent参数封装了需要启动的目标Activity信息。

   - Service：当需要启动一个Service时，可调用Context的startService(Intent intent)或bindService(Intent service,ServiceConnection conn,int flags)方法，这两个方法中的Intent参数封装了需要启动的目标Service信息

   - BroadcastReceiver：当需要触发一个BroadcastReceiver时，可调用Context的sendBroadcast(Intent intent)、sendStickyBroadcast(Intent intent)或sendOrderedBroadcast(Intent intent,String receiverPermission)方法来发送广播消息，这三个方法中的Intent参数封装了需要触发目标BroadcastReceiver的信息。

     ***注***：Intent封装了当前组件需要启动或触发的目标组件的信息，当一个组件通过Intent表示了启动或触发另一个组件的“意图”之后，这个意图可分为两类：

   - 显式Intent：显式Intent明确指定了需要启动或者触发的组件类名。

   - 隐式Intent：隐式Intent只是指定需要启动或者触发的组件应满足怎样的条件。

   > 　　对于显式Intent而言，Android系统无须对该Intent做任何解析，系统直接找到指定的目标组件启动或触发它即可。

   > 　　对于隐式Intent而言，Android系统需要对该Intent进行解析，解析出它的条件，然后再去系统中查找与之匹配的目标组件去启动或触发他们。

   ​		Android通过IntentFilter来判断被调用组件是否符合隐式Intent的条件，被调用组件可通过IntentFilter来声明自己所满足的条件（也就是声明自己到底能处理那些隐式Intent）。

3. **BroadcastReceiver**：借助广播接收器组件，系统能够在常规用户流之外向应用传递事件，从而允许应用响应系统范围内的广播通知。由于广播接收器是另一个明确定义的应用入口，因此系统甚至可以向当前未运行的应用传递广播。例如，应用可通过调度提醒来发布通知，以告知用户即将发生的事件。而且，通过将该提醒传递给应用的广播接收器，应用在提醒响起之前即无需继续运行。许多广播均由系统发起，例如，通知屏幕已关闭、电池电量不足或已拍摄照片的广播。应用也可发起广播，例如，通知其他应用某些数据已下载至设备，并且可供其使用。尽管广播接收器不会显示界面，但其可以[创建状态栏和通知](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html)，在发生广播事件时提醒用户。但广播接收器更常见的用途只是作为通向其他组件的*通道*，旨在执行极少量的工作。例如，它可能会根据带[JobScheduler](https://developer.android.google.cn/reference/android/app/job/JobScheduler.html)的事件调度[JobService](https://developer.android.google.cn/reference/android/app/job/JobService.html)来执行工作

4. **ContentProvider**：内容提供程序管理一组共享的应用数据，您可以将这些数据存储在文件系统、SQLite 数据库、网络中或者您的应用可访问的任何其他持久化存储位置。其他应用可通过内容提供程序查询或修改数据（如果内容提供程序允许）。例如，Android 系统可提供管理用户联系人信息的内容提供程序。因此，任何拥有适当权限的应用均可查询内容提供程序(如[ContactsContract.Data](https://developer.android.google.cn/reference/android/provider/ContactsContract.Data.html))，以读取和写入特定人员的相关信息。我们很容易将内容提供程序看作数据库上的抽象，因为其内置的大量 API 和支持时常适用于这一情况。但从系统设计的角度看，二者的核心目的不同。对系统而言，内容提供程序是应用的入口点，用于发布由 URI 架构识别的已命名数据项。因此，应用可以决定如何将其包含的数据映射到 URI 命名空间，进而将这些 URI 分发给其他实体。反之，这些实体也可使用分发的 URI 来访问数据。在管理应用的过程中，系统可以执行以下特殊操作：

   - 分配 URI 无需应用保持运行状态，因此 URI     可在其所属的应用退出后继续保留。当系统必须从相应的 URI 检索应用数据时，系统只需确保所属应用仍处于运行状态。
   - 这些 URI 还会提供重要的细粒度安全模型。例如，应用可将其所拥有图像的 URI 放到剪贴板上，但将其内容提供程序锁定，以便其他应用程序无法随意访问它。当第二个应用尝试访问剪贴板上的 URI 时，系统可允许该应用通过临时的 *URI 授权*来访问数据，这样便只能访问 URI 后面的数据，而非第二个应用中的其他任何内容。
   - 内容提供程序也适用于读取和写入您的应用不共享的私有数据

5. **Service**：服务是一个通用入口点，用于因各种原因使应用在后台保持运行状态。它是一种在后台运行的组件，用于执行长时间运行的操作或为远程进程执行作业。服务不提供界面。例如，当用户使用其他应用时，服务可能会在后台播放音乐或通过网络获取数据，但这不会阻断用户与 Activity 的交互。诸如 Activity 等其他组件可以启动服务，使该服务运行或绑定到该服务，以便与其进行交互。事实上，有两种截然不同的语义服务可以告知系统如何管理应用：已启动服务会告知系统使其运行至工作完毕。此类工作可以是在后台同步一些数据，或者在用户离开应用后继续播放音乐。在后台同步数据或播放音乐也代表了两种不同类型的已启动服务，而这些服务可以修改系统处理它们的方式：

   - 音乐播放是用户可直接感知的服务，因此，应用会向用户发送通知，表明其希望成为前台，从而告诉系统此消息；在此情况下，系统明白它应尽全力维持该服务进程运行，因为进程消失会令用户感到不快。
   - 通常，用户不会意识到常规后台服务正处于运行状态，因此系统可以更自由地管理其进程。如果系统需要使用     RAM 来处理用户更迫切关注的内容，则其可能允许终止服务（然后在稍后的某个时刻重启服务）。

> 绑定服务之所以能运行，原因是某些其他应用（或系统）已表示希望使用该服务。从根本上讲，这是为另一个进程提供 API 的服务。因此，系统会知晓这些进程之间存在依赖关系，所以如果进程 A 绑定到进程 B 中的服务，系统便知道自己需使进程 B（及其服务）为进程 A 保持运行状态。此外，如果进程 A 是用户关心的内容，系统随即也知道将进程 B 视为用户关心的内容。由于存在灵活性（无论好坏），服务已成为非常有用的构建块，并且可实现各种高级系统概念。动态壁纸、通知侦听器、屏幕保护程序、输入方法、无障碍功能服务以及众多其他核心系统功能均可构建为在其运行时由应用实现、系统绑定的服务。

### Android应用程序结构

`src`：存放程序的源代码

`gen`：系统自动生成，无需手动修改。最重要的是R.java文件，保存了程序中所用到的所有控件和资源ID；

`asscts`：存放不进行编译加工的原生文件，这里的资源文件不会在R.java自动生成ID；

`drawable-hdpi`:存放高分辨率的资源图片；

`drawable-ldpi`：存放低分辨率的资源图片；

`drawable-mdpi`：存放中等分辨率的资源图片；

`drawable-xhdpi`：存放超高分辨率的资源图片，从Android2.2才开始增加的分类；

`layout`：存放项目的布局文件，就是应用程序的xml文件；

`menu`：菜单文件，同样为xml格式，在此为可以为应用程序添加菜单；

`values`：该目录中存放的xml文件，定义了各种类型的key-value键值对。一般有dimens、string、styles、colors、arraye等，通常程序中共用到尺寸、字符串值、样式、颜色、数组等都在该文件中定义，便于使用和修改；

`AndroidMainfest.xml`：这是程序的清单文件。应用程序中所用到的所有组件，都要在该文件中注册，否则程序无法识别，不能使用。

