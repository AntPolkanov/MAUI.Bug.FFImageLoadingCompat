# MAUI.Bug.FFImageLoadingCompat
The repo reproduces an exception on Android in the Release mode.
Works fine in the Debug mode on Android and on iOS in both modes.

The issue is only happing with the .NET 8-preview6. Works fine on .NET 7

Steps:
1. Build the project in Release
2. Deploy to an Android device (I used emulators and Samsung Note 8)
3. Run

**Expected result:** a page appears with the dotnet_bot image as a placeholder which then replaced by an image downloaded from the internet.

**Actual result:**  the app crashes with an error. Here is [tobmstone_20](https://1drv.ms/u/s!AsNErTB5et9zzXJyn8j9WHyeknWO?e=VtFyx7). Logcat:
```
08-03 09:28:49.672  9110  9110 E .ffimageloading: Not starting debugger since process cannot load the jdwp agent.
08-03 09:28:50.537  9110  9143 E OpenGLRenderer: Unable to match the desired swap behavior.
08-03 09:28:50.564  9110  9148 F libc    : Fatal signal 11 (SIGSEGV), code 2 (SEGV_ACCERR), fault addr 0x7421a73000 in tid 9148 (.NET TP Worker), pid 9110 (.ffimageloading)
08-03 09:28:51.068  9154  9154 F DEBUG   : *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
08-03 09:28:51.068  9154  9154 F DEBUG   : Build fingerprint: 'google/sdk_gphone64_arm64/emu64a:14/UPB3.230519.006/10229193:userdebug/dev-keys'
08-03 09:28:51.068  9154  9154 F DEBUG   : Revision: '0'
08-03 09:28:51.068  9154  9154 F DEBUG   : ABI: 'arm64'
08-03 09:28:51.068  9154  9154 F DEBUG   : Timestamp: 2023-08-03 09:28:50.608506737+1000
08-03 09:28:51.068  9154  9154 F DEBUG   : Process uptime: 2s
08-03 09:28:51.068  9154  9154 F DEBUG   : Cmdline: com.companyname.maui.bug.ffimageloading
08-03 09:28:51.068  9154  9154 F DEBUG   : pid: 9110, tid: 9148, name: .NET TP Worker  >>> com.companyname.maui.bug.ffimageloading <<<
08-03 09:28:51.068  9154  9154 F DEBUG   : uid: 10209
08-03 09:28:51.068  9154  9154 F DEBUG   : tagged_addr_ctrl: 0000000000000001 (PR_TAGGED_ADDR_ENABLE)
08-03 09:28:51.068  9154  9154 F DEBUG   : pac_enabled_keys: 000000000000000f (PR_PAC_APIAKEY, PR_PAC_APIBKEY, PR_PAC_APDAKEY, PR_PAC_APDBKEY)
08-03 09:28:51.068  9154  9154 F DEBUG   : signal 11 (SIGSEGV), code 2 (SEGV_ACCERR), fault addr 0x0000007421a73000
08-03 09:28:51.068  9154  9154 F DEBUG   :     x0  b400007421a72fff  x1  00000000778ce00c  x2  000000000000001e  x3  000000000000001e
08-03 09:28:51.068  9154  9154 F DEBUG   :     x4  00000000778ce02a  x5  b400007421a7301d  x6  0000000000000000  x7  b4000074819a07e0
08-03 09:28:51.068  9154  9154 F DEBUG   :     x8  00000000778ce000  x9  00000000778ce000  x10 0000000000000000  x11 000000725203e840
08-03 09:28:51.068  9154  9154 F DEBUG   :     x12 0000000000000000  x13 0000000000000002  x14 0000000000000008  x15 b4000074719d30f8
08-03 09:28:51.068  9154  9154 F DEBUG   :     x16 00000072cd810a70  x17 000000757c8d09c0  x18 0000007236a66000  x19 000000000000001e
08-03 09:28:51.068  9154  9154 F DEBUG   :     x20 b400007421a72fff  x21 0000000000000000  x22 b40000730194a6f0  x23 00000000778ce000
08-03 09:28:51.068  9154  9154 F DEBUG   :     x24 0000000070fde190  x25 0000000070fb1de0  x26 00000072cda16000  x27 00000072466f6000
08-03 09:28:51.068  9154  9154 F DEBUG   :     x28 0000000000000001  x29 00000072466f3e70
08-03 09:28:51.068  9154  9154 F DEBUG   :     lr  00000072cd6717b8  sp  00000072466f3df0  pc  000000757c8d09ec  pst 0000000020001000
08-03 09:28:51.068  9154  9154 F DEBUG   : 14 total frames
08-03 09:28:51.068  9154  9154 F DEBUG   : backtrace:
08-03 09:28:51.068  9154  9154 F DEBUG   :       #00 pc 00000000000529ec  /apex/com.android.runtime/lib64/bionic/libc.so (__memcpy_aarch64_simd+44) (BuildId: 584bea18de667f24c490a5df46125bc8)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #01 pc 00000000006717b4  /apex/com.android.art/lib64/libart.so (art::JNI<false>::GetByteArrayRegion(_JNIEnv*, _jbyteArray*, int, int, signed char*)+352) (BuildId: 9fbb49c28046dcdb656b256563f6944c)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #02 pc 0000000000515f7c  /system/lib64/libhwui.so (JavaInputStreamAdaptor::read(void*, unsigned long)+200) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #03 pc 00000000005c8804  /system/lib64/libhwui.so ((anonymous namespace)::FrontBufferedStream::read(void*, unsigned long) (.__uniq.259476571162685252206752541612201816602)+184) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #04 pc 00000000005c88ec  /system/lib64/libhwui.so ((anonymous namespace)::FrontBufferedStream::peek(void*, unsigned long) const (.__uniq.259476571162685252206752541612201816602.8f1c1c36362eb42cefaa8ed6ceaf4bc1)+48) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #05 pc 0000000000271f80  /system/lib64/libhwui.so (SkCodec::MakeFromStream(std::__1::unique_ptr<SkStream, std::__1::default_delete<SkStream> >, SkCodec::Result*, SkPngChunkReader*, SkCodec::SelectionPolicy)+116) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #06 pc 0000000000270a8c  /system/lib64/libhwui.so (doDecode(_JNIEnv*, std::__1::unique_ptr<SkStreamRewindable, std::__1::default_delete<SkStreamRewindable> >, _jobject*, _jobject*, long, long) (.__uniq.26938061605105508016343812100800822394)+740) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #07 pc 0000000000270708  /system/lib64/libhwui.so (nativeDecodeStream(_JNIEnv*, _jobject*, _jobject*, _jbyteArray*, _jobject*, _jobject*, long, long) (.__uniq.26938061605105508016343812100800822394)+232) (BuildId: 2caff52cdd7985a5dd5f0639d675ef9b)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #08 pc 0000000000329790  /data/misc/apexdata/com.android.art/dalvik-cache/arm64/boot.oat (art_jni_trampoline+176)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #09 pc 00000000006f8cc4  /data/misc/apexdata/com.android.art/dalvik-cache/arm64/boot.oat (android.graphics.BitmapFactory.decodeStream+484)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #10 pc 0000000000350080  /apex/com.android.art/lib64/libart.so (art_quick_invoke_static_stub+640) (BuildId: 9fbb49c28046dcdb656b256563f6944c)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #11 pc 00000000005151ac  /apex/com.android.art/lib64/libart.so (art::JNI<false>::CallStaticObjectMethodA(_JNIEnv*, _jclass*, _jmethodID*, jvalue const*)+832) (BuildId: 9fbb49c28046dcdb656b256563f6944c)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #12 pc 0000000000041da4  /data/app/~~RItoneN68pso2GFWpoZQBQ==/com.companyname.maui.bug.ffimageloading-oaT1k_S4pC2qEgGTTFhyNw==/split_config.arm64_v8a.apk!libmono-android.release.so (offset 0x90a000) (java_interop_jnienv_call_static_object_method_a+48) (BuildId: 61a34325905bcc5cf15047bd8242ba339c24fc4a)
08-03 09:28:51.068  9154  9154 F DEBUG   :       #13 pc 0000000000007200  <anonymous:7573bf0000>
08-03 09:28:51.078   204   204 E tombstoned: Tombstone written to: tombstone_20
08-03 09:28:51.116   380   380 E BpTransactionCompletedListener: Failed to transact (-32)
08-03 09:28:51.138  3078  3919 E OpenGLRenderer: Unable to match the desired swap behavior.
08-03 09:28:51.146  2421  3081 E OpenGLRenderer: Unable to match the desired swap behavior.
08-03 09:28:52.908   920  3060 E TaskPersister: File error accessing recents directory (directory doesn't exist?).
```
