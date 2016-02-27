## WORK EXPERIENCES
#### Python/MaxScript Integration for 3ds Max, 3ds Max Team, Autodesk.
_2015.4 - 2016.2_  
_Tech involved: C/C++, Python, Maxscript, Scintilla editing component._

* <a name="3dsmax_adsk_1"></a>Implemented a mechanism on its "script listener" UI that allows the user to switch between _python_ and _maxscript_ modes easily. undo/redo feature supported.

    > * Massive legacy max code plus haphazard Scintilla lib transplanting left me a ugly and tangled entry-point, making it difficult to wedge a new feature.
    > * TODO there is a note explaining my work.

* <a name="3dsmax_adsk_2"></a>Translated _maxscript_'s ["Context Expressions"](http://help.autodesk.com/view/3DSMAX/2016/ENU/?guid=__files_GUID_E672728A_EE15_4197_9EDD_487781167B01_htm) to the _python_'s counterpart using its "with-yield" statement.

    > * Some context expression items in maxscript(such as _"at time"_ and _"at level"_) were not fully exposed as APIs, hence wrapping with "with-yield" directly was insufficient.
    > * A in-depth forging way was needed. The Python Extended Module was used for us to customize the underlying behavior. How to manager the recursive calling (a "with" within another 'with") and write a generic interface for most items fitting the condition was the difficult point.

And,

* I took the IELTS exam in Aug, 2015 and gained a overall band score of 6.5.

#### Stingray Editor Development for Stingray game engine, Game Group Team, Autodesk.
_2014.4 - 2015.4_  
_Tech involved: C#, C++, QT, Chromium-CEF, AngularJS/Bootstrap, javascript/html5/canvas/css3._

* <a name="stingray_adsk_1"></a>Integrated Autodesk in-house products to Stingray Editor.

    > * Stingray Editor is a 3D game editor and is a part of [Stingray Game Engine](www.autodesk.com/products/stingray).
    > * [CIP](http://www.autodesk.com/company/customer-involvement-program), [CER](https://knowledge.autodesk.com/support/autocad/troubleshooting/caas/sfdcarticles/sfdcarticles/Customer-Error-Reporting-CER.html) are the in-house products for Autodesk to gather customer feedbacks, count usage details, and report issues when the product encounters an fatal crash.
    > * Stingray Editor is realized under a C/S architecture. How to deploy MC3 and CER between the front-end and the back-end was the point (either side can be crashed and disconnected without notifying the other side).

* <a name="stingray_adsk_2"></a>Implemented the _Progress Bar_ widget for Stingray Editor.

    > * As stated above, two ends were developed separately. The front-end was a html5 page driven by AngularJS framework and decorated by Bootstrap css lib, residing in a special QT widget extended by Chromium-CEF. The back-end was implemented by C#, the actual logic of how progress bar performs is written here. The back-end module, as a public service, takes charge of interacting with other modules.
    > * The two ends communicate with each other through both http and web socket, as well as by underlying IPC (for sharing viewport contents from engine side). Benefiting from AngularJS's dynamic-binding ability as well as our talent work for marshaling data, the different type of data can stream back and forth seamlessly without writing any extra code for extracting and converting them.
    > * The difficulty here was how to coordinate multiple incoming tasks. Firstly we tried making it as a threading mechanism but that brought a lot complicity. At last we changed it as an asynchronous way using C#'s event handler mechanism.

* <a name="stingray_adsk_3"></a>Implemented the _Color Picker_ Panel for Stingray Editor.

    > * It's a front-end work of html5 web app, based on canvas drawing and AngularJS framework, as stated above.
    > * the point here was how to sync the final color value when users operate with various component of this widget.
    > * TODO there is a note explaining my work.

And,

* During this stage I've been learning English by myself, targeting IELTS.

#### Beast Lighting Renderer Integration for MayaLT, Game Group Team, Autodesk.
_2013.10 - 2014.4_  
_Tech involved: C++, Graphics knowledge_

* <a name="beast_adsk"></a>Implemented Tessellation Lighting Map for Displacement Map of MayaLT.

    > * [Beast](http://www.autodesk.com/products/beast/) is a middleware  focusing on realtime GI lighting rendering. It had a plugin on MayaLT. My mission was to integrate one notable feature "Beast's tessellation algorithm" with MayaLT's built-in Displacement Map.
    > * Displacement Map produces only as necessary as possible _control points_ due to cost concerns, which is yet not enough for a GI-level lighting map rendering. For this reason a tessellation processing was needed before sending vertices to pipes.
    > * The original algorithm from Beast team didn't refine rough vertex points, which means for a single point there are 3 vertices, 3 normals and 3 uvs corresponding to it, a simple but brutal way to bear new tessellated points.
    > * My improvement for such was to store points using _indices way_ instead, meaning we can save a lot spaces hence. The difficult point was how to produce an appropriate normal for each new-tessellated points.

#### Feature Development for Flame/Smoke, Creative Finishing Team, Autodesk.
_2011.2 - 2013.10_  
_Tech involved: C++, Scons, gdb, Linux devel platform, Graphics knowledge_

* <a name="flame_adsk_1"></a>Fixed a series of tangled bugs resulting from "Reeler" UI positioning bias of Flame.
    > * [Flame](http://www.autodesk.com/products/flame-family)/[Smoke](http://www.autodesk.com/products/smoke) are a bundle of creative finishing tools for visual effects, focusing on post compositing and film cutting and running on Linux/Mac. There is a UI widget named "Reeler" on its desktop.
    > * After some complicated code refactor and new feature being mixed, a series of weird and tangled regression defects around "Reeler Positioning" began to keep popping up. I took a long time to track the source and fixed them systematically. The difficulty is its code base is huge, ancient and grew up unhealthy(due to the history issues). How to survive this code disaster was a key point for me to became a veteran of working in a large project.

* <a name="flame_adsk_2"></a>Investigated a memory management issue for Flame/Smoke.

    > * Flame works on Linux and Smoke runs on Mac Os X, and both them need to cope with thousands of millions of small geometry elements. This refers to the memory management issues on OS. we had a weird problem about (re)allocating/releasing mem during the time, and the behaviors were different on Linux and Mac.
    > * Eventually I basically got the answer after digging into them for a while. During the time I asked a [ question](http://stackoverflow.com/questions/15529643/what-does-malloc-trim0-really-mean) on the stackoverflow.
    > * TODO there is a note related to this issue.

* <a name="flame_adsk_3"></a>Implemented _[Action Replica Node](https://knowledge.autodesk.com/search-result/caas/CloudHelp/cloudhelp/2016/ENU/Flame/files/GUID-0E1E86A5-310B-4F1F-A9C1-97E64A896AAB-htm.html)_ for Flame.

    > * Action nodes provide in-context access to a fully functional Action module. And The Replica node in Action allows the user to create multiple instances of most Action objects (including the Replica node itself). Think of Replica as a macro that saves users the time of creating multiple Axis nodes with mimic links and individual offsets.
    > * The point here was the code base of Flame/Smoke didn't design for such a feature from stem to stern.I had to modify the very underlying architecture so as to extend such a basis. After that all the instances managed by _Replica Node_ share one geometry structure and only other higher level nodes (like position, rotation, scales) upon it differ with each other by offset argument.The underlying geometry structure contain a countable pointer (similar to shared_ptr) with them to manage the life-time when needed to cope with _Replica Node_.

#### 3D Feature Devel for Mobile Games, 3D R&D Team, IN-FUN Corp.
_2010.10 - 2011.2_  
_Tech involved: C Language, a 3D game engine made in-house, Graphics knowledge._
* <a name="3d_infun"></a>Developed a lightweight 3D game running on a MTK mobile platform.

    > * The company uses an in-house 3D developing engine from MTK corp. The point was the language running on it only can be C. That was really a crude experience. During the time I learned how to write an object-oriented program using C.


#### Feature Devel for NetGames, Client-side Team, ShenXue Corp.
_2010.4 - 2010.10_  
_Tech involved: C++, Ogre 3D._

* Attended 3D game logic development.
* Sorry I can't recall too much details since so a long time has passed :(

#### Boot Camp Training, 3D R&D Team, Ourgame Corp.
_2009.7 - 2010.4_  
_Tech involved: C++, Ogre 3D._

* Fixed bugs, touched Ogre 3D as well as general graphics knowledge.
* Sorry I can't recall too much details since so a long time has passed :(
