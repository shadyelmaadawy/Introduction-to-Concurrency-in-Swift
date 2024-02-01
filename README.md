# Introduction to Concurrency in Swift

HEY HEY HEY, Hooowww are you mates!! **hope things go well with you**, I told you, when I am **feeling Brrr, I am turned on!**

What do we have for today? **A problem I see In a lot of projects without care about**, and not in one place, It's In **a lot of places**.

Dude, It’s very important to care about your software **performance**, **stability**, and **scalability**, always keep this in your mind 

> Your software represents you.
> 
> Shadi El-Maadawy

Let's leave the **excess philosophy behind** and explain **concurrency**!

## What is that?

**Concurrency / Multithreading**, or as I like to define it, how to become an **OCTOPUS!**

Yeah, that Is correct, your software will have **a lot of arms the same as an octopus**! 

You can fetch **data from servers**, do **background backups**, and In the scene, you will make the **sky sparkle**!

:- But how?? **are you messing with me ya shadi**?? If we make a **huge loop, the app will freeze**! how I can do **all of these things together**?!

= Dude! It's **operating system magic**!
When you deal with **multithreading**, you will find you can do almost anything.

:- Okaaay how? 

= Let me tell you ; )

First, you need to know what is **multithreading**, every app has **one thread**, called **Main Thread**, 

When you open an app It's created by default, and all of your operations ( **User interface events** - **Http request Invoking**, **Database operations** ) .. etc, all of them called **In this thread**.

That's why you will face a lot of **performance issues**, Main-Thread ( **for me** ) should be used to handle **user interface events and response for it**, nothing else!

Okay, how I can do **other operations**?

First, there is **DispatchQueue** and **NSOperation** (  **More customizable** ), but without going into deep details, Simply, you need first to understand what is ( **DispatchQueue** ) and QOS ( **Quality of service** ). 

## DispatchQueue: 

What Is that? 

An engine LOL, and apply **FIFO concept**, I will give it a task, and It will execute it, 

Depended on **Global queue** or **Main queue** ( **Main-Thread** ), **rememeber**? 

Also, you can create your queue, It can be **serially** or **concurrently**, but let’s leave these things behind for now.

## Global Queue:

Created by **system** and you add **tasks inside it to be executed** - with the required **quality of service** 

And In a **concurrent execution**, most of the **magic** is **located here in this queue**!

## QOS - Quality of service

**Qos is types of threads** used in making **concurrency operations in global queue**, There is a lot, As example:

 1. **User-Initiated**:  
     - . I like to use It for 
         - Http request Invokeing, 
         - Database Operations, 
         -  "Things need time to process" 
 2. **Background**: 
     - مش محتاجة يعني

## Some actions:

First I need to make a http request, and I will just **send a request In global queue**  with **userInitiated QOS**
```swift
    DispatchQueue.global(qos: .userInitiated).async {
    
        // MARK: - Http Request code
        sleep(10) // Delay for ten seconds, act as http request
        
    }
```

Now my app will doing the **required task** away from **Main-Thread**, **and when request finished and I want to update the view I will doing that**

```swift
    DispatchQueue.main.async {
        self.textLabel.stringValue = "Hello-World!;"
    }
```

**No magic, very simple**, you can apply the same with **Rx-Swift**, look here: [#](https://github.com/shadyelmaadawy/Bosta-OSX-Menu-Bar-Agent/blob/master/BostaAgent/Core/Network%20Layer/Core/Custom/URLSessionHttpEngine.swift#L65)

 ```swift
    .observe(on: SerialDispatchQueueScheduler.init(qos: .userInitiated))
```


I make the request In **RX-Queue** with **userInitiated QOS**, and when the data **returned** and "**processed**", I returned It in **Main-Thread** [#](https://github.com/shadyelmaadawy/Bosta-OSX-Menu-Bar-Agent/blob/master/BostaAgent/Core/Network%20Layer/Core/Root/NetworkEngine.swift#L57)
 ```swift
    .observe(on: MainScheduler.instance)
```

## Tip:

If you have global variable and alot of threads will access it ( **set or get** ), You need to lock it with  **Thread lock, as example NSLock**
```swift

    let multithreadLock = NSLock.init()
    multithreadLock.withLock {
        // Access to global variable
    }
    multithreadLock.unlock()    
```
## Resources

1. [DispatchQueue](https://developer.apple.com/documentation/dispatch/dispatchqueue)
2. [APPLE.com - Quality Of
    Service](https://developer.apple.com/documentation/dispatch/dispatchqos)
 2. [An Innovative OSX-Menu Bar
    App used concurrecny With Rx-Swift](https://github.com/shadyelmaadawy/Bosta-OSX-Menu-Bar-Agent/tree/master)
 2. [An Innovative IOS
    App used concurrecny With Combine](https://github.com/shadyelmaadawy/Utopia-Crypto-Payments-Gateway/tree/master)

## To Infinity and beyond!
Multithreading is like **magic**, you can do a lot of things with it, , But everything **comes with a cost**, one **mistake your app will crush** without a **warning**!, 

I like **multithreading** a lot, I think I will talk about It and **explain more details In a series**!

Stay tuned!✨

## Credits

### Copyright (©) 2024, Shady K. Maadawy, All rights reserved. 
 [@shadudiix](https://github.com/shadyelmaadawy) 💫
