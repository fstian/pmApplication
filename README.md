# pmApplication
修改源码静默安装

修改源码：1：添加权限
        --- a/frameworks/base/core/res/AndroidManifest.xml
+++ b/frameworks/base/core/res/AndroidManifest.xml
@@ -2451,6 +2451,10 @@
     <p>Not for use by third-party applications. -->
     <permission android:name="android.permission.INSTALL_PACKAGES"
         android:protectionLevel="signature|privileged" />
+    <permission android:name="android.permission.HIDE_INSTALL_PACKAGES"
+        android:protectionLevel="normal" />
+    <permission android:name="android.permission.HIDE_UNINSTALL_PACKAGES"
+        android:protectionLevel="normal" />
           2：修改IPackageManger.Stub
        
     --- a/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
+++ b/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
@@ -11704,7 +11704,13 @@ public class PackageManagerService extends IPackageManager.Stub {
     public void installPackageAsUser(String originPath, IPackageInstallObserver2 observer,
             int installFlags, String installerPackageName, int userId) {
         android.util.SeempLog.record(90);
-        mContext.enforceCallingOrSelfPermission(android.Manifest.permission.INSTALL_PACKAGES, null);
+        //mContext.enforceCallingOrSelfPermission(android.Manifest.permission.INSTALL_PACKAGES, null);
+        if(mContext.checkCallingPermission(android.Manifest.permission.HIDE_INSTALL_PACKAGES) == PackageManager.PERMISSION_GRANTED) {
+            Slog.i(TAG, "installerPackageName: checkCallingPermission "+installerPackageName);
+        } else {
+            Slog.i(TAG, "installerPackageName: checkCallingPermission PERMISSION_DENIED"+PackageManager.PERMISSION_DENIED);
+            mContext.enforceCallingOrSelfPermission(android.Manifest.permission.INSTALL_PACKAGES, null);
+        }
 
