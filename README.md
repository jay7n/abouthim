#YOUCHUN LI

##ABOUT HIM
Youchun Li, (or Jayson Li in English),  
was born on Jan 9th, 1985,  
and lives in Shanghai, China.  

* jay7n.li@outlook.com
* github.com/jay7n
* www.cnblogs.com/lookof
* +86 13761843450

##EDUCATION
* Bachelor of Computer Since and Technology,  
  [Hunan Institute Of Science And Technology](http://www.hnist.cn)  
  _graduated in Jun, 2009_

* Language Certificate:
    * IELTS (Academic) Overall Band Score: 6.5  
      _obtained in Aug, 2015._  

##OBJECTIVE/INTERESTS
* Goal in 2016:
    * Mobile(iOS) devel. and/or
    * Web(full stack) devel. and/or
    * In-depth graphics exploring. and/or
    * As a manager lead a small scrum team. and/or
    * An opportunity working/living aboard. and/or
    * High level design architect devel. and/or
    * System low level devel, such like:
        * memory allocation.
        * performance improvement.
        * computing job/task scheduling.
        * parallel/concurrency.
    * And/or could touch one or more new languages(to me) below:
        * Go
        * Swift
        * Ruby

    All the fields listed above are the ones I'm not familiar with too much, **but that's the reason** why I'm so into them recently. Feed me.  

    And,
    * Would like work under Mac OSX / Unix-like OS.

##EXPERTISE/HIGHLIGHTS
* Uses C/C++, C#, Lua, Python, Js/h5/css3, Bash/fish, (well and) Markdown as daily skills.
* Uses _bash/fish + vim-like editor + git_ as daily developing toolchains.
* Used to focus on OpenGL/DX, [general (realtime/offline) graphics knowledge](http://www.cnblogs.com/lookof/category/220911.html).(TODO add more notes there)
* Tried AngularJS, Node, Swift Language for a while before.
* Playing with Tornado, MongoDB right now in my toy project: [LinWords](https://github.com/jay7n/LinWords)

##EXPERIENCES
#### Python/MaxScript Integration for 3ds Max, 3ds Max Team, Autodesk.
_2015.4 - 2016.2_
* Implemented a mechanism on its "script listener" UI that allows the user to switch between _python_ and _maxscript_ modes easily. undo/redo feature supported.
    > * Massive legacy max code plus haphazard Scintilla lib transplanting left me a ugly and tangled entry-point, making it difficult to wedge a new feature.
    > * TODO there is a note explaining my work.

* Translated _maxscript_'s ["Context Expressions"](http://help.autodesk.com/view/3DSMAX/2016/ENU/?guid=__files_GUID_E672728A_EE15_4197_9EDD_487781167B01_htm) to the _python_'s counterpart using its "with-yield" statement.
    > * Some context expression items in maxscript(such as _"at time"_ and _"at level"_) were not fully exposed as APIs, hence wrapping with "with-yield" directly was insufficient.
    > * A in-depth forging way was needed. The Python Extended Module was used for us to customize the underlying behavior. How to manager the recursive calling (a "with" within another 'with") and write a generic interface for most items fitting the condition was the difficult point.

* Tech involved: C/C++, Python, Maxscript, Scintilla editing component.

And,

* I took the IELTS exam in Aug, 2015 and gained a overall band score of 6.5.

#### Stingray Editor Development for Stingray game engine, Game Group Team, Autodesk.
_2014.4 - 2015.4_
* Integrated Autodesk in-house products to Stingray Editor.
    > * Stingray Editor is a 3D game editor and is a part of [Stingray Game Engine](www.autodesk.com/products/stingray).
    > * [CIP](http://www.autodesk.com/company/customer-involvement-program), [CER](https://knowledge.autodesk.com/support/autocad/troubleshooting/caas/sfdcarticles/sfdcarticles/Customer-Error-Reporting-CER.html) are the in-house products for Autodesk to gather customer feedbacks, count usage details, and report issues when the product encounters an fatal crash.
    > * Stingray Editor is realized under a C/S architecture. How to deploy MC3 and CER between the front-end and the back-end was the point (either side can be crashed and disconnected without notifying the other side).

* Implemented the _Progress Bar_ widget for Stingray Editor.
    > * As stated above, two ends were developed separately. The front-end was a html5 page driven by AngularJS framework and decorated by Bootstrap css lib, residing in a special QT widget extended by Chromium-CEF. The back-end was implemented by C#, the actual logic of how progress bar performs is written here. The back-end module, as a public service, takes charge of interacting with other modules.
    > * The two ends communicate with each other through both http and web socket, as well as by underlying IPC (for sharing viewport contents from engine side). Benefiting from AngularJS's dynamic-binding ability as well as our talent work for marshaling data, the different type of data can stream back and forth seamlessly without writing any extra code for extracting and converting them.
    > * The difficulty here was how to coordinate multiple incoming tasks. Firstly we tried making it as a threading mechanism but that brought a lot complicity. At last we changed it as an asynchronous way using C#'s event handler mechanism.
* Implemented the _Color Picker_ Panel for Stingray Editor.
    > * It's a front-end work of html5 web app, based on canvas drawing and AngularJS framework, as stated above.
    > * the point here was how to sync the final color value when users operate with various component of this widget.
    > * TODO there is a note explaining my work.

* Tech involved: C#, C++, QT, Chromium-CEF, AngularJS/Bootstrap, javascript/html5/canvas/css3.

And,

* During this stage I've been learning English by myself, targeting IELTS.

#### Beast Lighting Renderer Integration for MayaLT, Game Group Team, Autodesk.
_2013.10 - 2014.4_
* Implemented Tessellation Lighting Map for Displacement Map of MayaLT.
    > * [Beast](http://www.autodesk.com/products/beast/) is a middleware  focusing on realtime GI lighting rendering. It had a plugin on MayaLT. My mission was to integrate one notable feature "Beast's tessellation algorithm" with MayaLT's built-in Displacement Map.
    > * Displacement Map produces only as necessary as possible _control points_ due to cost concerns, which is yet not enough for a GI-level lighting map rendering. For this reason a tessellation processing was needed before sending vertices to pipes.
    > * The original algorithm from Beast team didn't refine rough vertex points, which means for a single point there are 3 vertices, 3 normals and 3 uvs corresponding to it, a simple but brutal way to bear new tessellated points.
    > * My improvement for such was to store points using _indices way_ instead, meaning we can save a lot spaces hence. The difficult point was how to produce an appropriate normal for each new-tessellated points.
    
* Tech involved: C++

#### Feature Development for Flame/Smoke, Creative Finishing Team, Autodesk.
_2011.2 - 2013.10_
* Fixed a series of tangled bugs resulting from "Reeler" UI positioning bias of Flame.
    > * [Flame](http://www.autodesk.com/products/flame-family)/[Smoke](http://www.autodesk.com/products/smoke) are a bundle of creative finishing tools for visual effects, focusing on post compositing and film cutting and running on Linux/Mac. There is a UI widget named "Reeler" on its desktop.
    > * After some complicated code refactor and new feature being mixed, a series of weird and tangled regression defects around "Reeler Positioning" began to keep popping up. I took a long time to track the source and fixed them systematically. The difficulty is its code base is huge, ancient and grew up unhealthy(due to the history issues). How to survive this code disaster was a key point for me to became a veteran of working in a large project.

* Investigated a memory management issue for Flame/Smoke
* Implemented "Replica" Node for Flame/Smoke.
* Tech involved: C++, Scons, gdb, Linux devel platform

#### 3D Feature Devel for Mobile Games, 3D R&D Team, IN-FUN Corp.
_2010.10 - 2011.2_
* Developed a lightweight 3D game running on a MTK mobile platform.
* Tech involved: C Language, a 3D game engine made in-house

#### Feature Devel for NetGames, Client-side Team, ShenXue Corp.
_2010.4 - 2010.10_
* Attended 3D game logic development.
* Sorry I can't recall too much details since a long time has passed :(
* Tech involved: C++, Ogre 3D.

#### Boot Camp Training, 3D R&D Team, Ourgame Corp.
_2009.7 - 2010.4_
* Fixed bugs, touched Ogre 3D as well as general graphics knowledge.
* Sorry I can't recall too much details since a long time has passed :(
* Tech involved: C++, Ogre 3D.


##TASTE
* Mac OSX / Unix-like OS fans. anti-Windows.
* Editor(emacs/vim/sublime/atom) + Command-line(bash/fish) fans. anti-IDE.
* Favorite sites: [github.com](), [stackoverflow.com](), [news.ycombinator.com](), [zhihu.com](), [ipn.li](), [xkcd.com]()
* Hobbies: working out, playing guitar, brewing coffee.
