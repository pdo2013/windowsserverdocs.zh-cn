找到 HGS 保护者证书。 你将需要一个签名证书和加密证书以初始化 HGS 群集。
向 HGS 提供证书的最简单方法是创建受密码保护的 PFX 文件为每个证书，其中包含公钥和私钥的密钥。 如果使用由 HSM 支持的密钥或其他非可导出的证书，请确保在继续之前的证书安装到本地计算机证书存储。
若要使用的证书的详细信息，请参阅[获取证书的 HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs)。

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->