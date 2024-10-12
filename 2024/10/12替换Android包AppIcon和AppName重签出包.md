1. 解包APK

   ```shell
   apktool d your_app.apk -o output_folder
   ```

   如果提示apktool命令找不到，用brew安装一下

   ```shell
   brew install apktool
   ```

2. 替换App Icon 和 App 名称（具体过程略）

3. 重新打包APK

   ```shell
   apktool b output_folder -o new_app.apk
   ```

4. 使用`zipalign`优化APK

   ```shell
   zipalign -v 4 new_app.apk zipaligned_app.apk
   ```

5. 使用`apksigner`签名APK

   ```shell
   apksigner sign \
       --ks your-keystore-file \
       --ks-pass pass:your-keystore-password \
       --key-alias your-keystore-alias \
       --key-pass pass:your-key-password \
       --out signed_app.apk zipaligned_app.apk
   ```

   `--ks your-keystore-file`：指定你的 keystore 文件。

   `--ks-pass pass:your-keystore-password`：keystore 的密码。

   `--key-alias your-keystore-alias`：你在 keystore 中使用的密钥别名。

   `--key-pass pass:your-key-password`：与指定的密钥别名相关联的密码。

   `--out signed_app.apk`：输出的签名 APK 文件名。

   `zipaligned_app.apk`：待签名的 APK 文件。

6. 验证签名

   ```shell
   apksigner verify signed_app.apk
   ```

   使用 `apksigner` 验证签名，以确保 APK 正确签名，如果没有任何输出，则说明签名成功。

7. 安装APK

   ```shell
   adb install signed_app.apk
   ```

   最后能够成功安装就表示大功告成。