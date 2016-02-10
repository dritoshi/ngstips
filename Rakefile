require "rubygems"
require "bundler/setup"
require "stringex"

desc "Convert gitbook to octopress"
task :build do
  sh "gitbook build"
  # sh "cp -a _book ../catway2/source/bioinformatics"
  sh "rsync -av _book/ ../catway2/source/bioinformatics/"
end