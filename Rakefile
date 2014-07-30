# どんなCファイルあって、どこで入手できるなどがわかりませんので、
# とりあえず、Rakefileを書いてみた。
# 内容としては、srcディレクトリにある.c Cソースファイルを各自の.o オブジェクトファイルに変換し、
# 最後全ての.oファイルでhoge.exeというファイルを作成し、runタスクを行うこと。
# cleanをすることで、更新時の依存関係を綺麗にする(一度削除してからだから)

CC = "gcc" # コンパイルコマンド
TARGET = "hoge.o" # ファイル名を変数に
INC_DIR ="include" # include ディレクトリを変数に	

require 'rake/clean' # 5,6行はクリーニング
CLEAN.include(["src/*.o", "*.exe"]) # CLOBBERもokかな

#desc "Clean & Run"
#task :all => ["clean", "run"] # タスク2つがある

desc "Run build"
task :default => "run" #デフォルトタスクはrun

desc "Build Main"
task :run => "hoge.o" do # まずrunを作成するにはhogeファイルが必要
  puts "---- run  ----" # 見やすくするため
  sh "./#{TARGET}" # TARGETのhoge.exeを集めてくれる
end

SRCS = FileList["src/*.c"] # srcディレクトリ以下にある全ての.cファイルを指定	
OBJS = SRCS.ext('o') 
# .oファイルも必要ですが、これから.cで作るので、今定義しても起こらない。
# 20行で.cファイルを使って.oファイルを生成してから21行(.o)を行う。

file "hoge.o" => OBJS do |t| # OBJSの.oファイルでhogeファイルを生成
  puts "---- link ----" # 見やすくするため
  sh "#{CC} -o #{t.name} #{t.prerequisites.join(' ')}" 
  # hogeファイルに必要のsrcディレクトリにある.oファイルをコンパイル
end

rule '.o' => '.c' do |t| # ルール: 全ての.oは.cで生成される(ここはhoge.exeに必要な.oを生成)
  puts "---- compile ----" # 見やすくするため
  sh "#{CC} -I#{INC_DIR} -c #{t.source} -o #{t.name}"
  # .cで.oを作るルール: CCコンパイラでINC_DIRディレクトリに.cで.oを作る
end