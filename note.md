lib/main.dart ফাইলটি হলো পুরো অ্যাপের হৃৎপিণ্ড। একজন ফ্রন্টএন্ড ওয়েব ডেভেলপারের চোখে একে একদম সহজ বাংলায় লাইন-বাই-লাইন ভেঙে নিচে বিস্তারিত আলোচনা করা হলো:

অংশ ১: ইমপোর্ট এবং অ্যাপ শুরু (main ফাংশন)
dart
import 'package:flutter/material.dart';
void main() {
  runApp(const MyApp());
}

  

  
import 'package:flutter/material.dart';
  
  

  
যেমনভাবে React-এ আমরা import React from 'react' করি, তেমনি Flutter-এ গুগল-এর ম্যাটেরিয়াল ডিজাইন সিস্টেম (বাটন, কার্ড, ফন্ট, থিম ইত্যাদি) ব্যবহার করতে এই প্যাকেজটি ইমপোর্ট করা হয়।

  

  
void main():
  
  

  
ব্রাউজারে জাভাস্ক্রিপ্ট স্ক্রিপ্ট নিজে নিজেই ওপর থেকে এক্সিকিউট হওয়া শুরু করে, কিন্তু C/Java/Dart-এর মতো ভাষায় একটি নির্দিষ্ট শুরুর বিন্দু (Entry Point) দরকার হয়—আর সেটিই হলো এই main() ফাংশন।

  

  
runApp(const MyApp()):
  
  

  
React-এ যেমন আমরা লিখি: createRoot(rootElement).render(<App />)

  
ঠিক তেমনই Flutter-এ runApp() বলে দেয়: "স্ক্রিনে এখন MyApp নামের উইজেট ট্রি মাউন্ট (Mount) করো।"

  
const কেন? যদি কোনো উইজেটের ডাটা ভবিষ্যতে রানটাইমে কখনো না বদলায়, তাকে const লিখলে Flutter মেমোরিতে নতুন অবজেক্ট না বানিয়ে আগেরটাই রি-ইউজ করে (পারফরম্যান্স বুস্ট)।

  

  
অংশ ২: মূল অ্যাপ উইজেট (MyApp - StatelessWidget)
dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(colorScheme: .fromSeed(seedColor: Colors.deepPurple)),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

  

  
StatelessWidget কী?
  
  

  
এটি এমন একটি কম্পোনেন্ট যার নিজের কোনো অভ্যন্তরীণ পরিবর্তনশীল স্টেট (State) নেই। অর্থাৎ সাধারণ Dumb/Presentational Component।

  

  
build(BuildContext context):
  
  

  
React-এর render() মেথড বা ফাংশনাল কম্পোনেন্টের রিটার্ন ব্লকের মতো।

  
context কী? উইজেট ট্রিতে এই নির্দিষ্ট উইজেটটি কোথায় দাঁড়িয়ে আছে তার লোকেশন বা ঠিকানা। এর মাধ্যমেই প্যারেন্টের থিম বা স্ক্রিনের সাইজ ইত্যাদি জানা যায়।

  

  
MaterialApp:
  
  

  
এটি আপনার পুরো অ্যাপের কনফিগারেশন রুট (যেমন ওয়েব অ্যাপে গ্লোবাল Context Provider বা Router Provider)।

  
theme: অ্যাপের গ্লোবাল কালার স্কিম ও ফন্ট ডিরেক্টরি।

  
home: অ্যাপ চালু হলে সবার প্রথমে কোন স্ক্রিন/পেজটি দেখাবে (Home Route)। এখানে MyHomePage পেজ দেওয়া হয়েছে।

  

  
অংশ ৩: পেজ এবং স্টেট উইজেট (MyHomePage - StatefulWidget)
dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});
  final String title;
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

ওয়েবে আমরা একটাই ফাংশনে স্টেট ও এইচটিএমএল লিখি, কিন্তু Flutter-এ StatefulWidget-কে ২টি ক্লাসে ভাগ করা হয়:


  

  
MyHomePage (উইজেট কনফিগারেশন):
  
  

  
এটি বাইরে থেকে প্রপস (Props) গ্রহণ করে। যেমন: title।

  
এটি ইমিউটেবল (Immutable) বা অপরিবর্তনশীল।

  

  
createState():
  
  

  
এটি বলে দেয়: "এই উইজেটের স্টেট এবং ইউআই হ্যান্ডেল করার দায়িত্বে থাকবে _MyHomePageState ক্লাস।"

  

  
অংশ ৪: স্টেট ও লজিক (_MyHomePageState)
dart
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0; // State variable
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  

  
int _counter = 0;:
  
  

  
React-এর ভাষায়: const [counter, setCounter] = useState(0);

  

  
_incrementCounter() এবং setState():
  
  

  
আপনি যদি সরাসরি _counter++ করতেন, মেমোরিতে ভ্যালু বাড়ত কিন্তু স্ক্রিনে কিছুই আপডেট হতো না।

  
যখন আপনি setState(() { _counter++; }) কল করেন, Flutter সাথে সাথে বুঝে নেয়: "এই পেজের স্টেটে পরিবর্তন এসেছে, তাই দ্রুত build() ফাংশনটি আবার রান করে স্ক্রিন রি-রেন্ডার করো!"

  

  
অংশ ৫: স্ক্রিনের ইউআই গঠন (Scaffold ও লেআউট)
dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title), // প্যারেন্ট থেকে আসা টাইটেল (Props)
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: .center,
          children: [
            const Text('You have pushed the button this many times:'),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}

এখানে ব্যবহৃত উইজেটগুলোর ওয়েব সমতুল্য রূপ:


  

  
Scaffold:
  
  

  
সাধারণ মোবাইল স্ক্রিনের একটা রেডিমেড লেআউট কঙ্কাল। এর ভেতর appBar (টপবার/হেডার), body (মূল কন্টেন্ট) এবং floatingActionButton (নিচের ডানপাশের গোল বাটন) বসানোর সরাসরি স্লট থাকে।

  

  
AppBar:
  
  

  
ওয়েবের <header> বা <nav>। এখানে widget.title দিয়ে প্যারেন্ট উইজেট থেকে পাঠানো টাইটেল প্রিন্ট করা হয়েছে।

  

  
Center:
  
  

  
এটি তার ভেতরের চাইল্ডকে অনুভূমিক ও উল্লম্বভাবে স্ক্রিনের ঠিক মাঝখানে নিয়ে আসে।

  

  
Column:
  
  

  
CSS-এর সাথে সরাসরি তুলনা:
  
css
display: flex;
flex-direction: column;
justify-content: center; /* mainAxisAlignment: MainAxisAlignment.center */
align-items: center;     /* crossAxisAlignment: CrossAxisAlignment.center */

  

  
এর ভেতর children: [...] একটি অ্যারে বা লিস্ট নেয়, যেখানে পরপর উইজেটগুলো উপর থেকে নিচে বসে।

  

  
Text('$_counter'):
  
  

  
Dart-এর স্ট্রিং ইন্টারপোলেশন (যেমন জাভাস্ক্রিপ্টে Template Literal: `${counter}`)।

  

  
FloatingActionButton:
  
  

  
নিচে ভাসমান প্লাস (+) আইকন বাটন। যখন ইউজার ক্লিক করে (onPressed), তখন _incrementCounter মেথডটি রান হয়ে কাউন্ট ১ বাড়িয়ে দেয়।

  

  
সারসংক্ষেপ ফ্লো-চার্ট
main() 
  └── runApp()
        └── MyApp (MaterialApp তৈরি করে)
              └── MyHomePage (Stateful স্ক্রিন)
                    └── _MyHomePageState
                          ├── State: _counter
                          ├── Method: _incrementCounter() -> setState()
                          └── UI: Scaffold
                                ├── AppBar (হেডার)
                                ├── Body: Center -> Column -> Text
                                └── FloatingActionButton (বাটন)