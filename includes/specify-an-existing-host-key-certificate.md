或者，可以指定一个指纹，如果想要使用你自己的证书。 这可以是你想要在多台计算机之间共享证书或使用证书绑定到 TPM 或 HSM 的情况下很有用。 下面是创建 TPM 绑定证书 （这可以防止被盗和另一台计算机上使用私钥并需要仅 TPM 1.2） 的示例：

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->