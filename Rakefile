require 'rubygems'
require 'cucumber'
require 'cucumber/rake/task'
require 'parallel'
require 'json'

@browsers = JSON.load(open('browsers.json'))
@parallel_limit = ENV["nodes"] || 1
@parallel_limit = @parallel_limit.to_i

task :cucumber do
  Parallel.map(@browsers, :in_threads => @parallel_limit) do |browser|
    begin
      puts "Running with: #{browser.inspect}"
      ENV['BROWSER'] = browser['browser']
      ENV['BROWSER_VERSION'] = browser['browser_version']
      ENV['OS'] = browser['os']
      ENV['OS_VERSION'] = browser['os_version']

      Rake::Task[:run_features].execute()
    rescue Exception => e
      puts "Failed to run tests for #{browser.inspect}"
    end
  end
end

Cucumber::Rake::Task.new(:run_features)
task :default => [:cucumber]
