# MAUI.Bug.FFImageLoadingCompat
The repo reproduces an exception on Android in the Release mode.
Works fine in the Debug mode on Android and on iOS in both modes.

The bug is supposedly caused by the .NET 8-preview6. Works fine on .NET 7

Steps:
1. Build the project in Release
2. Deploy to an Android device (I used Samsung Note 8)
3. Run

**Expected result:** a page appears with the dotnet_bot image as a placeholder which then replaced by an image downloaded from the internet.

**Actual result:**  the app crashes with the following error in logcat:
```
07-27 17:42:46.245 14676 14702 F libc    : Fatal signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x7e77c00000 in tid 14702 (.NET TP Worker), pid 14676 (.ffimageloading)
07-27 17:42:46.313 14723 14723 F DEBUG   : *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
07-27 17:42:46.314 14723 14723 F DEBUG   : Build fingerprint: 'samsung/greatltexx/greatlte:9/PPR1.180610.011/N950FXXUGDVG5:user/release-keys'
07-27 17:42:46.314 14723 14723 F DEBUG   : Revision: '9'
07-27 17:42:46.314 14723 14723 F DEBUG   : ABI: 'arm64'
07-27 17:42:46.314 14723 14723 F DEBUG   : pid: 14676, tid: 14702, name: .NET TP Worker  >>> com.companyname.maui.bug.ffimageloading <<<
07-27 17:42:46.314 14723 14723 F DEBUG   : signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x7e77c00000
07-27 17:42:46.314 14723 14723 F DEBUG   :     x0  0000007e77bfffd1  x1  0000000000000000  x2  0000000000000000  x3  0000000000000000
07-27 17:42:46.314 14723 14723 F DEBUG   :     x4  0000000000000000  x5  0000007e77c00015  x6  0000000000000000  x7  0000000000000000
07-27 17:42:46.314 14723 14723 F DEBUG   :     x8  0000000000000000  x9  0000000000000000  x10 0000000000000000  x11 0000000000000000
07-27 17:42:46.314 14723 14723 F DEBUG   :     x12 0000000000000000  x13 0000000000000000  x14 0000007e9e0d49e4  x15 0000007e9d7ba848
07-27 17:42:46.314 14723 14723 F DEBUG   :     x16 0000007e9e1b1108  x17 0000007f2245ed30  x18 0000000000000000  x19 0000000000000044
07-27 17:42:46.314 14723 14723 F DEBUG   :     x20 0000000000000000  x21 0000007e77bfffd1  x22 0000007e9787ce00  x23 000000007a756000
07-27 17:42:46.314 14723 14723 F DEBUG   :     x24 0000007e97920000  x25 0000007e9e0e5111  x26 0000007e9e0e4843  x27 0000007e78dfd588
07-27 17:42:46.314 14723 14723 F DEBUG   :     x28 0000000000000043  x29 0000007e78dfb050
07-27 17:42:46.314 14723 14723 F DEBUG   :     sp  0000007e78dfaf90  lr  0000007e9def3cb8  pc  0000007f2245edfc
07-27 17:42:46.414 14723 14723 F DEBUG   :
07-27 17:42:46.414 14723 14723 F DEBUG   : backtrace:
07-27 17:42:46.414 14723 14723 F DEBUG   :     #00 pc 000000000001ddfc  /system/lib64/libc.so (memcpy+204)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #01 pc 0000000000381cb4  /system/lib64/libart.so (art::JNI::GetByteArrayRegion(_JNIEnv*, _jbyteArray*, int, int, signed char*)+812)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #02 pc 000000000014ef94  /system/lib64/libandroid_runtime.so (JavaInputStreamAdaptor::doRead(void*, unsigned long, _JNIEnv*)+140)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #03 pc 000000000014eda0  /system/lib64/libandroid_runtime.so (JavaInputStreamAdaptor::read(void*, unsigned long)+96)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #04 pc 000000000044b104  /system/lib64/libhwui.so (FrontBufferedStream::bufferAndWriteTo(char*, unsigned long)+68)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #05 pc 000000000044b298  /system/lib64/libhwui.so (FrontBufferedStream::read(void*, unsigned long)+264)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #06 pc 000000000044b174  /system/lib64/libhwui.so (FrontBufferedStream::peek(void*, unsigned long) const+52)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #07 pc 000000000017f854  /system/lib64/libhwui.so (SkCodec::MakeFromStream(std::__1::unique_ptr<SkStream, std::__1::default_delete<SkStream>>, SkCodec::Result*, SkPngChunkReader*)+84)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #08 pc 0000000000149cd4  /system/lib64/libandroid_runtime.so (doDecode(_JNIEnv*, std::__1::unique_ptr<SkStreamRewindable, std::__1::default_delete<SkStreamRewindable>>, _jobject*, _jobject*)+1004)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #09 pc 000000000014b5d4  /system/lib64/libandroid_runtime.so (nativeDecodeStream(_JNIEnv*, _jobject*, _jobject*, _jbyteArray*, _jobject*, _jobject*)+140)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #10 pc 0000000000428ff0  /system/framework/arm64/boot-framework.oat (offset 0x420000) (android.graphics.BitmapFactory.nativeDecodeStream [DEDUPED]+256)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #11 pc 000000000087dde0  /system/framework/arm64/boot-framework.oat (offset 0x420000) (android.graphics.BitmapFactory.decodeStream+272)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #12 pc 0000000000562a4c  /system/lib64/libart.so (art_quick_invoke_static_stub+604)
07-27 17:42:46.414 14723 14723 F DEBUG   :     #13 pc 00000000000d0160  /system/lib64/libart.so (art::ArtMethod::Invoke(art::Thread*, unsigned int*, unsigned int, art::JValue*, char const*)+232)
07-27 17:42:46.415 14723 14723 F DEBUG   :     #14 pc 0000000000468a78  /system/lib64/libart.so (art::(anonymous namespace)::InvokeWithArgArray(art::ScopedObjectAccessAlreadyRunnable const&, art::ArtMethod*, art::(anonymous namespace)::ArgArray*, art::JValue*, char const*)+104)
07-27 17:42:46.415 14723 14723 F DEBUG   :     #15 pc 0000000000469740  /system/lib64/libart.so (art::InvokeWithJValues(art::ScopedObjectAccessAlreadyRunnable const&, _jobject*, _jmethodID*, jvalue*)+408)
07-27 17:42:46.415 14723 14723 F DEBUG   :     #16 pc 000000000035eb54  /system/lib64/libart.so (art::JNI::CallStaticObjectMethodA(_JNIEnv*, _jclass*, _jmethodID*, jvalue*)+636)
07-27 17:42:46.415 14723 14723 F DEBUG   :     #17 pc 0000000000041da4  /data/app/com.companyname.maui.bug.ffimageloading-MAWEPhH-8itZaaF54mBgpw==/split_config.arm64_v8a.apk (offset 0x90a000) (java_interop_jnienv_call_static_object_method_a+48)
07-27 17:42:46.415 14723 14723 F DEBUG   :     #18 pc 000000000000706c  <anonymous:0000007f1ef00000>
07-27 17:42:46.620  3620  3620 E /system/bin/tombstoned: Tombstone written to: /data/tombstones/tombstone_08
07-27 17:42:46.626  3557  3557 E audit   : type=1701 audit(1690443766.621:18062): auid=4294967295 uid=10338 gid=10338 ses=4294967295 subj=u:r:untrusted_app:s0:c82,c257,c512,c768 pid=14676 comm=2E4E455420545020576F726B6572 exe="/system/bin/app_process64" sig=11
07-27 17:42:46.645  3368  3368 E lmkd    : Error writing /proc/14676/oom_score_adj; errno=22
07-27 17:42:46.646 14729 14729 E Zygote  : isWhitelistProcess - Process is Whitelisted
07-27 17:42:46.647 14729 14729 E Zygote  : accessInfo : 1
07-27 17:42:46.655 14729 14729 E ng.android.loo: Not starting debugger since process cannot load the jdwp agent.
07-27 17:42:46.693  3932  4114 E InputDispatcher: channel 'e15a361 com.companyname.maui.bug.ffimageloading/crc647020217f456fd405.MainActivity (server)' ~ Channel is unrecoverably broken and will be disposed!
07-27 17:42:46.702  3369  4703 E NativeSemDvfsGpuManager: acquire:: Start
07-27 17:42:46.702  3369  4703 E NativeSemDvfsGpuManager: acquire:: timeout = 1000 mIsAcquired = 0  mTagName : SurfaceFlinger
07-27 17:42:46.703  3369  4703 E NativeCustomFrequencyManager: [NativeCFMS] BpCustomFrequencyManager::requestGPU()
07-27 17:42:46.703  3369  4703 E NativeSemDvfsGpuManager: acquire:: End
07-27 17:42:46.708  3932  4269 E WindowManager: win=Window{d01f3bd u0 Splash Screen com.companyname.maui.bug.ffimageloading EXITING} destroySurfaces: appStopped=false win.mWindowRemovalAllowed=true win.mRemoveOnExit=true win.mViewVisibility=0 caller=com.android.server.wm.AppWindowToken.destroySurfaces:871 com.android.server.wm.AppWindowToken.destroySurfaces:852 com.android.server.wm.WindowState.onExitAnimationDone:5443 com.android.server.wm.-$$Lambda$01bPtngJg5AqEoOWfW3rWfV7MH4.accept:2 java.util.ArrayList.forEach:1262 com.android.server.wm.AppWindowToken.onAnimationFinished:2413 com.android.server.wm.AppWindowToken.setVisibility:552
07-27 17:42:46.740  3932  3975 E WindowManager: RemoteException occurs on reporting focusChanged, w=Window{e15a361 u0 com.companyname.maui.bug.ffimageloading/crc647020217f456fd405.MainActivity EXITING}
07-27 17:42:46.740  3932  3975 E WindowManager: android.os.DeadObjectException
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.os.BinderProxy.transactNative(Native Method)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.os.BinderProxy.transact(Binder.java:1145)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.view.IWindow$Stub$Proxy.windowFocusChanged(IWindow.java:500)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at com.android.server.wm.WindowState.reportFocusChangedSerialized(WindowState.java:3951)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at com.android.server.wm.WindowManagerService$H.handleMessage(WindowManagerService.java:5523)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.os.Handler.dispatchMessage(Handler.java:106)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.os.Looper.loop(Looper.java:214)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at android.os.HandlerThread.run(HandlerThread.java:65)
07-27 17:42:46.740  3932  3975 E WindowManager: 	at com.android.server.ServiceThread.run(ServiceThread.java:44)
```
