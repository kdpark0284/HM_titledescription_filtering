# Clean & Group Titles for HM

Coded for Colab

# Installation

1. Insert codes and run
[[
%%bash
apt-get update
apt-get install g++ openjdk-8-jdk python-dev python3-dev
pip3 install JPype1
pip3 install konlpy
pip install openpyxl
pip install rapidfuzz

%env JAVA_HOME "/usr/lib/jvm/java-8-openjdk-amd64"
]]
2. Next to the First Line, insert this code and run
[[
! git clone https://github.com/SOMJANG/Mecab-ko-for-Google-Colab.git

cd Mecab-ko-for-Google-Colab

!bash install_mecab-ko_on_colab_light_220429.sh

cd /content/mecab-ko-dic-2.1.1-20180720

!pip install jamo

from jamo import h2j, j2hcj
def get_jongsung_TF(sample_text):
    sample_text_list = list(sample_text)
    last_word = sample_text_list[-1]
    last_word_jamo_list = list(j2hcj(h2j(last_word)))
    last_jamo = last_word_jamo_list[-1]
    jongsung_TF = "T"
    if last_jamo in ['ㅏ', 'ㅑ', 'ㅓ', 'ㅕ', 'ㅗ', 'ㅛ', 'ㅜ', 'ㅠ', 'ㅡ', 'ㅣ', 'ㅘ', 'ㅚ', 'ㅙ', 'ㅝ', 'ㅞ', 'ㅢ', 'ㅐ,ㅔ', 'ㅟ', 'ㅖ', 'ㅒ']:
        jongsung_TF = "F"
    return jongsung_TF
def make_user_dic_csv(morpheme_type, word_list, user_dic_file_name):
  file_data = []
  for word, score in word_list:
    jongsung_TF = get_jongsung_TF(word)
    line = f"{word},,,{score},{morpheme_type},,{jongsung_TF},{word},,,,,\n"
    # 단어, left-ID, right-ID, Weight, 품사, 의미분류, 종성유무, 읽기, 타입, 첫번째품사, 마지막품사, 표현
    # https://openuiz.blogspot.com/2018/12/mecab-ko-dic.html
    file_data.append(line)
  with open("./user-dic/user-nnp.csv", 'w', encoding='utf-8') as f:
    for line in file_data:
      f.write(line)

word_list = [('1TB', 0), ('2TB', 0),  ('미패드',0), ('Wi-Fi',0), ('다크레드',0), ('스타라이트',0), ('그라파이트', 0), ('미드나이트', 0), ('블루에어', 0), ('뮤패드', 0),
              ('미스티라일락', 0), ('보타닉그린',0), ('코튼플라워', 0),('시나모롤',0), ('그레이그린', 0), ('펄 화이트', 0),('블루', 0),('미스트 블루', 0), ('그래비티 그레이',0)]
#  단어 추가 방법: ('원하는단어형태', 0) 를 대괄호 안에 넣어주세요. ( )엔 하나의 단어만 사용해주세요.
#  Method to add a word: Insert ('desired_word_form', 0) inside the bracket. Use only one word inside the parentheses.
#  Example) word_list = [('1TB', 0), ('2TB', 0)]
make_user_dic_csv(morpheme_type="NNP", word_list=word_list, user_dic_file_name='user-nnp.csv')
with open("./user-dic/user-nnp.csv", 'r', encoding='utf-8') as f:
  file_new = f.readlines()
file_new
!bash autogen.sh
!make
!sudo make install
!bash tools/add-userdic.sh
]]
