###################################################################################################################
####                                     Hi1711 chip solution�̼�SOCǩ������                                   ####
####                            ��Ҫ����������ļ���HASH׼�����ߡ�ǩ�����ߣ���SHELL�ű�ʵ��                    ####
###################################################################################################################

һ���ؼ��ļ�˵����
	xx_rsa_4096.cfg��        RSA4096ǩ�������ļ���xx:l0fw,l1fw,u-boot��
	xx_rsa_2048.cfg��        RSA2048ǩ�������ļ���xx:l0fw,l1fw,u-boot������������
	prepare_code_sign_data�� HASH׼�����ߣ����ڼ�����̼���hash
	generate_sign_image��    ǩ�����ߣ������������յĹ̼�


һ��ǩ�����裺
	1. ���� ./prepare_code_sign_data xx.bin 1(version) (l0fw,l1fw,u-boot) ���� xx_hash_sign.bin
	2. ��xx_hash_sign.bin����ǩ������ǩ������xx_hash_sign.bin.rsa ���ҽ� rsacert.cer�ŵ���ǰĿ¼
	3. ���� ./generate_sign_image xx_rsa_4096.cfg �������յ�xx_rsa_4096.bin
	
����xxx.cfg�����ļ��޸�˵��
	�����ļ���һ���޸����ڵ�ǩ��˫ǩ���߲���ǩ���������ļ����ɣ������ֶοͻ������޸ģ�������Ҫ�������汾˵����
	�޸�Ϊǩ��ģʽ���ֶ���Ҫ��FW_SECTION_LIST��FW_SECTION_FILES���޸�ʾ�����£�
	1. ����ǩ����
		export FW_SECTION_LIST="padding padding padding padding padding padding"
		export FW_SECTION_FILES="padding padding padding padding padding padding"
	2. ��ǩ��
		export FW_SECTION_LIST="host_root_pubk host_subkey_cert host_code_sign padding padding padding"
		export FW_SECTION_FILES="rsa_host_rtpubk_4096_C.bin rsacert.cer l0fw_hash_sign.bin.rsa padding padding padding"
	3. ˫ǩ��
		export FW_SECTION_LIST="host_root_pubk host_subkey_cert host_code_sign guest_root_pubk guest_subkey_cert guest_code_sign"
		export FW_SECTION_FILES="rsa_host_rtpubk_4096_C.bin rsacert.cer l1fw_hash_sign.bin.rsa rsa_guest_rtpubk_4096_C.bin rsacert.cer l1fw_hash_sign.bin.rsa"
		

����CMS���̸�֤��HASH���˵��
	CMS���̸�֤��HASH�����prepare_code_sign_data��ɣ����ڼ���̼�HASH֮ǰ���ã���֤���ļ��������λ�þ��ڸýű��ļ����޸ģ�
	1. ���λ��һ����l1fw.bin�汾һ���޸ģ��汾δ��˵���������޸ģ�
	2. ��֤���ļ����ͻ����Ը����Լ��ĸ�֤���ļ����������޸ġ�