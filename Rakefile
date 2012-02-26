require 'rake/clean'
require 'rake/testtask'

file 'ext/Makefile' do
  cd 'ext' do
    ruby "extconf.rb #{ENV['EXTOPTS']}"
  end
end

file 'ext/oil.so' => FileList.new('ext/Makefile', 'ext/oil.c') do
  cd 'ext' do
    sh 'make'
  end
end

desc 'Clean up Rubinius .rbc files.'
namespace :clean do
  task :rbc do
    system "find . -name *.rbc -delete"
  end
end

Rake::TestTask.new do |t|
  t.libs = ['ext']
  t.test_files = FileList['test/test*.rb']
end

CLEAN.add('ext/*{.o,.so,.log}', 'ext/Makefile')
CLOBBER.add('*.gem')

desc 'Build the gem'
task :gem do
  system "gem build oil.gemspec"
end

desc 'Compile the extension'
task :compile => "ext/oil.so"

task :test => :compile
task :default => :test