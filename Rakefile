$:.unshift 'lib'
require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'spec/rake/spectask'

task :default => "spec:suite"

begin
  require "jeweler"
  Jeweler::Tasks.new do |gemspec|
    gemspec.name        = "xlymian-redis-store"
    gemspec.summary     = "Rack::Session, Rack::Cache and cache Redis stores for Ruby web frameworks."
    gemspec.description = "Rack::Session, Rack::Cache and cache Redis stores for Ruby web frameworks."
    gemspec.email       = "guidi.luca@gmail.com"
    gemspec.homepage    = "http://github.com/xlymian/redis-store"
    gemspec.authors     = [ "Luca Guidi" ]
  end

  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler not available. Install it with: sudo gem install jeweler" 
end

namespace :spec do
  desc "Run all the examples by starting a detached Redis instance"
  task :suite do
    invoke_with_redis_cluster "spec:run"
  end

  Spec::Rake::SpecTask.new(:run) do |t|
    t.spec_files = FileList['spec/**/*_spec.rb']
    t.spec_opts = %w(-fs --color)
  end
end

desc "Run all examples with RCov"
task :rcov do
  invoke_with_redis_cluster "rcov_run"
end

Spec::Rake::SpecTask.new(:rcov_run) do |t|
  t.spec_files = FileList['spec/**/*_spec.rb']
  t.rcov = true
end

# courtesy of http://github.com/ezmobius/redis-rb team
load "tasks/redis.tasks.rb"
def invoke_with_redis_cluster(task_name)
  begin
    result = RedisClusterRunner.start_detached
    raise("Could not start redis-server, aborting.") unless result
    Rake::Task[task_name].invoke
  ensure
    RedisClusterRunner.stop
  end
end
