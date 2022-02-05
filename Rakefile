require 'fileutils'
require 'pathname'
require 'benchmark'

BUILD_DIR = Pathname.new("build")

USE_EXTENSIONS = false  # false # true
num_tests = 1000

desc "Generate temporary files"
task :generate do
  Rake::Task['clean'].execute
  FileUtils.mkdir_p BUILD_DIR
  methods = USE_EXTENSIONS ? 1 : num_tests
  extensions = USE_EXTENSIONS ? num_tests : 0
  method_body = "for i in 1...n { print(i + n); }; print(\"finished\")"
  File.open(BUILD_DIR + "main.swift", 'w') do |f|
    f.puts "class MyClass {"
    f.puts "var n = 1000"
    1.upto(methods) do |idx|
      f.puts "func method_#{idx}() { "
      f.puts method_body
      f.puts "}"
    end
    f.puts "}"
    f.puts
    1.upto(extensions) do |idx|
      f.puts "extension MyClass {"
      f.puts "func method_ext_#{idx}() {"
      f.puts method_body
      f.puts "}"
      f.puts "}"
    end
  end
end

desc "Run single compilation cycle"
task :compile do
  system("swiftc -Onone #{BUILD_DIR}/*.swift")
end

desc "Run multiple compilation cycle"
task :benchmark do
  [100, 1000, 2000, 3000, 5000, 10000].each do |tests|
    num_tests = tests
    puts "Using #{num_tests} #{USE_EXTENSIONS ? 'extensions' : 'methods'}"
    avg = 0
    num_runs = 3
    1.upto(num_runs) do |idx|
      Benchmark.bm do |b|
        Rake::Task["generate"].execute
        t = b.report("compile: ") { Rake::Task["compile"].execute }
        avg += t.total / num_runs 
      end
    end
    puts "Average compile time for n=#{tests} is #{avg.round(4)}"
  end
end

task :clean do
  FileUtils.rm_rf BUILD_DIR
  FileUtils.rm_rf "test"
end

