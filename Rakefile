require 'bundler/gem_tasks'
require 'rake/testtask'

desc 'Updates Manifest.txt with a list of files from git'
task :manifest do
  git_files = `git ls-files`.split("\n")
  ignored = %w(.gitignore Gemfile Gemfile.lock Manifest.txt knife-solo.gemspec script/newb script/test)

  File.open('Manifest.txt', 'w') do |f|
    f.puts (git_files - ignored).join("\n")
  end
end

task :release => :manifest

desc 'Renerates gh-pages from project'
task 'gh-pages' do
  require 'tmpdir'
  Dir.mktmpdir do |clone|
    clone = "local_tmp"; mkdir(clone)
    sh "git clone -b gh-pages git@github.com:matschaffer/knife-solo.git #{clone}"
    sh "rdoc -o #{clone}/doc"
  end
end

namespace :test do
  Rake::TestTask.new(:integration) do |t|
    t.libs << "test"
    t.test_files = FileList['test/integration/*_test.rb']
  end

  Rake::TestTask.new(:units) do |t|
    t.libs << "test"
    t.test_files = FileList['test/*_test.rb']
  end
end

desc "Alias for test:units"
task :test => ['test:units']
task :default => :test

