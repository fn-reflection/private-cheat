�{�����ł̋L�@
{{}}�͊e�X�ɂ���ĈقȂ�f�[�^������܂��B(�e���v���[�g�L�@)
�o�o���[���A�h���X�p�p :example@test.com

�閧���E���J���y�A�̍쐬
  ssh-keygen -t rsa -C "{{���[���A�h���X}}"
    ���̖��O��ύX����Ƃ��͐��������f�B���N�g���ɒ��ӂ��� (~/.ssh/�ł͂Ȃ�~�ɍ쐬�����)
    �ȉ��̗�ł�cd ./.ssh���Ă���id_rsa_github�ɖ��O��ύX�����Ƃ���
    �p�X�t���[�Y�͂���[11���ȏオ�D�܂���]

���J���̓��e���N���b�v�{�[�h�ɓ]��[windows]
  clip < id_rsa_github.pub
�z�X�e�B���O�T�[�r�X(Github)�Ɍ��J����o�^
  https://github.com/settings/keys
  �ɂČ��̖��O(�A�N�Z�X����[�������D�܂���)��clip�����f�[�^��o�^����

~/.ssh/config�t�@�C���̍쐬
Host github.com
  User git
  Hostname ssh.github.com
  Port 22
  IdentityFile "c:\Users\�o�o���O�C�����[�U�[���p�p\.ssh\id_rsa_github"

~/.ssh/config�t�@�C���̃V���{���b�N�����N�𐶐����ă��[�J����Git�v���O�����ɓǂݍ��܂���
mklink "C:\Program Files\Git\etc\ssh\ssh_config" "C:\Users\$user_profile\.ssh\config"
�@�@"C:\Program Files\Git\etc\ssh\ssh_config"�����݂���ꍇ�͊����̂��̂����l�[������

ssh�v���g�R���ɂ��ڑ��m�F
  ssh -T git@github.com