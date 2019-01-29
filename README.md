# Authenticated-Encryption-using-GCM
EVP接口支持执行经过身份验证的加密和解密的功能，以及将未加密的关联数据附加到邮件的选项。这种带有关联数据的认证加密（AEAD）方案通过加密数据来提供机密性，并且还通过在加密数据上创建MAC标签来提供真实性保证。MAC标签将确保数据在传输和存储期间不会被意外更改或恶意篡改。


# 认证加密介绍
Authenticated encryption with associated data (AEAD) is a variant of AE where the data to be encrypted needs both authentication and integrity as opposed to just integrity. AEAD binds associated data (AD) to the ciphertext and to the context where it's supposed to appear, so that attempts to "cut-and-paste" a valid ciphertext into a different context are detected and rejected.

It is required, for example, by network packets. The header needs integrity, but must be visible; payload, instead, needs integrity and also confidentiality. Both need authenticity.

# GCM认证加密程序流程
/ *创建并初始化上下文* / 
	if（！（ctx = EVP_CIPHER_CTX_new（）））handleErrors（）; 

	/ *初始化加密操作。* / 
	if（1！= EVP_EncryptInit_ex（ctx，EVP_aes_256_gcm（），NULL，NULL，NULL））
		handleErrors（）; 

	/ *如果默认12字节（96位）不合适，则设置IV长度* / 
	if（1！= EVP_CIPHER_CTX_ctrl（ctx，EVP_CTRL_GCM_SET_IVLEN，iv_len，NULL））
		handleErrors（）;

	/ *初始化键和IV * / 
	if（1！= EVP_EncryptInit_ex（ctx，NULL，NULL，key，iv））handleErrors（）; 

	/ *提供任何AAD数据。这可以被称为零次或多次
	 *必需
	 * / 
	if（1！= EVP_EncryptUpdate（ctx，NULL，＆len，aad，aad_len））
		handleErrors（）; 

	/ *提供要加密的消息，并获取加密输出。
	 *如果需要，可以多次调用EVP_EncryptUpdate 
	 * / 
	if（1！= EVP_EncryptUpdate（ctx，ciphertext，＆len，plaintext，plaintext_len））
		handleErrors（）; 
	ciphertext_len = len; 

	/ *完成加密。通常，密文字节可以在
	 此阶段写入，但在GCM模式下不会发生这种情况
	 * / 
	if（1！= EVP_EncryptFinal_ex（ctx，ciphertext + len，＆len））handleErrors（）; 
	ciphertext_len + = len; 

	/ *获取标签* / 
	if（1！= EVP_CIPHER_CTX_ctrl（ctx，EVP_CTRL_GCM_GET_TAG，16，tag））
		handleErrors（）; 

	/ *清理* / 
	EVP_CIPHER_CTX_free（ctx）; 
	
# 程序运行截图
![image](https://github.com/Ruipeng-LI/Authenticated-Encryption-using-GCM/blob/master/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20190129204128.png)
# 程序示列
可下载EVP_Authenticated_MFC.exe文件



