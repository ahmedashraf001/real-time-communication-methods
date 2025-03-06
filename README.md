# Quick summary/overview for real-time communication methods
<div dir="rtl">

<strong>الهدف :</strong> جعل السيرفر يقدر يبعت للclient بيانات بدون ما الclient يطلبها زي المعتاد انه بيبعت ريكوست للسيرفر بالحاجه ال عايزها منه ، لكن احنا عايزين الserver يقدر يبعت للclient في أي وقت براحته.
ودا عن طريق ان الclient بيسمح بدا (يعمل subscribe لevent معين على السيرفر مثلاً)
وعن طريق كمان بروتوكلات وطرق مختلفه تسمح بالنظام المختلف دا ، لأن العادي كان الhttp requests protocol وهو نظام request/response

 <br>

<strong>WebSockets :</strong> 
 <div dir="rtl"> 
 بيقدر الclient يطلب Handshake http request  للسيرفر  ولو وافق الريكوست دا بيتحولUpgrade  لWebsocket
و ال بيقدر يفتح اتصال دايم بين الclient وال server يسمح بنقل البيانات بينهم لحظياً من غير الحاجه ل ريكوست في كل مره.
وبيفضل الاتصال مفتوح طول مالطرفين متصلين.
 </div>
 <br>

<strong>Long Polling:</strong>
 <div dir="rtl"> 
 بيقدر الclient يطلب HTTP request للسيرفر ، والسيرفر يعلق الrequest عنده لحد مايحتاج يبعت للclient حاجه يقوم باعتها في الresponse بتاعت الrequest ، وتلقائي الclient اول مايستلم حاجه من السيرفر يبعتله http request تاني عشان يتعلق عند السيرفر.
 </div>
  <br>

<strong>SSE Server-Sent Events:</strong>
 <div dir="rtl"> 
 شبه websocket ولكن الفرق انه اتصال دائم في اتجاه واحدone way.
بيقدر الclient يطلب http request للسيرفر ، وSSE بيستخدم ميزةKeep Alive في HTTP/1.1 عشان يخلي الاتصال مفتوح بشكل دايم ، ولكن مشكلته انه one way السيرفر يقدر يبعت بيانات وقت مايحتاج للclient لكن الclient ميقدرش يبعت بيانات للسيرفر.
الخاصية بتعتمد على ان السيرفر مش بيبعت Content length للclient ، بالتالي الclient مش عارف length الريسبونس ال جايه عشان يقدر يقفل الconnection ، لذلك بيفضل الاتصال مفتوح والسيرفر كل ما يحتاج يبعت حاجه بيبعتها كpacket of data كأنها جزء من الresponse ال مش معروف ليها length ، ولكن الحقيقه ان الpacket دي response كامل أو حاجه كامله السيرفر عايز يبعتها.
 </div>
  <br>

<strong>AJAX Polling:</strong>
 <div dir="rtl"> 
 الclient بيبعت http request تلقائي كل فترة زمنيه ثابته( كل ثانيه مثلاً) للسيرفر ، والسيرفر لو عنده حاجه عايز يبعتها  في اي وقت للclient بيبعتها كresponse للريكوست دا ، ولو معندوش مش بيبعت حاجه.
 </div>
 <br>

# Problems 
<div dir="rtl"> 
<strong>مشكلة WebSockets :</strong>
 بيحتاج سيرفر بامكانيات معينه وعاليه وبيتعمله اعدادات خاصهconfigurations ، لأنه بيستهلك موارد السيرفر عشان يحافظ على اتصال دايم مفتوح لكل الclients مع السيرفر.
 </div>
 <br>
  <div dir="rtl"> 
<strong>مشكلة Long Polling:</strong>
 بيستهلك طلبات Http كتير حتى لو مفيش بيانات جديده عشان مدة تعليق الطلب في السيرفر لها حد اقصى وبيتقفل وبيتبعت تلقائي طلب تاني يتعلق.
 </div>
  <br>
  <div dir="rtl"> 
<strong>مشكلة SSE:</strong>
 انه one way يعني مينفعش فحاجه زي chat real time ، لأن السيرفر بس ال يقدر يبعت بيانات للclient وقت مايحتاج ، لكن ينفع في حاجه زي notifications real time.
 </div>
 <br>
 <div dir="rtl"> 
<strong>مشكلة AJAX Polling:</strong>
 بيستهلك طلبات Http كتير حتى لو مفيش بيانات جديده لأنه بيبعت كل مده زمنيه ثابته ، وفيه تأخير شويه لأنه معتمد على فترة زمنيه بين كل request وال بعده.
 </div>
</div>


# SignalR library

- SignalR is a real-time communication library that enables bi-directional communication between clients and servers depending on different real time methods.
- SignalR follows a three main layers architecture to provide flexibility and optimization in real-time communication

## <strong>Transport Layer (choosing method)</strong>
SignalR dynamically selects one of the following transport methods:
websocket, SSE, Long Polling
it determines the best available transport method based on some factors.

## <strong>Connection Layer (handle the connection)</strong>
Manage connect, reconnect
ensure stable connection between server and client.

## <strong>Hub Layer</strong>
Client and server connect each other through this hub layer.
The server can call client methods directly through the hub layer.
The client can call server methods directly through the hub layer.
