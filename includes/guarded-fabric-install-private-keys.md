如果您未加密或签名的第一个 HGS 服务器上的证书提供的 PFX 文件，则仅将公钥将复制到此服务器。
将需要通过导入到本地证书存储区，或在由 HSM 支持的密钥的情况下包含私钥的 PFX 文件中安装私钥配置密钥存储提供程序并将它与每个 HSM 制造商生产的证书相关联有关说明。

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->