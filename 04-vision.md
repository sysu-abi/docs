# 项目愿景
## 1、项目概要
赚闲钱是一个利用零碎时间帮助别人完成任务以获得利益的网络平台，更多的是面向在校学生。有那么一部分大学生在时间的分配上不太合理，
导致出现了一些零零碎碎的时间，而又有一些学生因为某些原因导致一些琐事无法完成。正是由于这种情况，我们的赚闲钱系统出现了。
在这里你可以以付出一些报酬的前提下发布一些你无法完成的琐事作为任务，同时也可以让零碎的时间不被太过于浪费而接取一些任务，以此来获得一些利益。
为了任务发布者以及接取任务者都能够有一个比较满意的体验，我们致力于站在用户的角度考虑以扩展系统的功能，以求在同类产品中占据优势。

## 2、项目背景
如今的市面上的赚闲钱系统，一部分是需要一部分本金的投资类赚闲钱系统；还有一部分是更加偏向于长时间兼职（像是家教、配送员）的赚闲钱系统。
即传统的赚闲钱系统并不是面对只有零碎时间以及只有琐碎问题的大学生。面对这些琐碎问题的学生只好求助于微信群等，但是这样不仅效率低下，而且群友的参与度不高。
综上所述，我们希望能够给有这两类问题的学生提供一个平台，既能让学生利用零碎时间获得利润，又能解决学生的部分琐碎问题，以此达到双赢的效果。

## 3、项目介绍
基本的挣闲钱机制
   - 首先是需要用户的注册与登录，再次就是需要用户的位置信息，因为如果是线下的任务（比如帮拿快递之类的）就需要任务发布者与接取者的距离很近，
   因此位置信息是必要的。
   - 为了减少虚假信息的存在，我们会引入[信用体制](https://github.com/sysu-abi/docs/blob/master/04-vision.md/#信用体制)，信用出现问题的用户，我们会给与一定时间的账号封禁作为惩罚。
   - 考虑到即使是这样的琐碎任务也会有难度上的不同，我们会让用户在发布任务的时候填写一个难度分级（根据工作量，截止时间等要素判断）。
   - 一个截取任务的用户界面，自然会伴随着任务的排序问题，什么样的任务放在醒目的位置，这排序也是一个问题。
   - 考虑到一些任务发布的信息不能概括，所以需要一个发布者与接受者交流的地方，但是又不能直接挂联系方式（个人信息问题，以及避免两者直接绕过平台线下交易）。
   - 对于信用系统，不能按时按量完成任务的自然是要扣信用分，同时我们暂定是推出每日任务系统已增加信用积分。
## 相关机制的具体说明

### 信用体制：
   每个新注册的用户都有一定的初始信用值，没有按时完成接取的任务会扣信用值，完成每日任务会加信用值，当然这里的每日任务必和用户发布的任务挂钩。信用值决定同一时间能够接取的任务数量，信用值越高的用户同一时间内能够接取的任务越多，这样在用户的能力范围内，一定时间获得的报酬也就越丰厚；信用值跌破一定的下限时将会对用户的账号实施一定的惩罚，比如一定时间内不能接取任务，或者直接封禁账号一段时间。