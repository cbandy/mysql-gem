require 'rake'
require 'rubygems'
require 'rake/clean'
require 'fileutils'
require 'date'
include FileUtils

CLEAN.include ["ext/*.{log,c,so,obj,pdb,lib,def,exp,manifest}", "ext/Makefile", "*.gem"]

name="mysql"
version="2.7.#{Date.today}".tr('-','.')

desc "Do everything, baby!"
task :default => [:package]

#task :package => [:clobber,:compile,:test,:makegem]
task :package => [:clean,:compile,:makegem]

desc "Compiles all extensions"
task :compile do
  cd "ext" do
    sh %{ ruby extconf.rb --with-mysql-include=C:/Progra~1/MySQL/MySQLS~1.0/include --with-mysql-lib=C:/Progra~1/MySQL/MySQLS~1.0/lib/opt }
    sh %{ nmake }
  end
end

desc "runs the given tests"
task :test do
  cd "ext" do
    # TODO: improve test setup so that tests pass with authentication
	ruby "test.rb", %{#{ENV["TESTOPTS"]}}
  end
end

desc "Build a binary gem for Win32"
task :makegem do
  spec = Gem::Specification.new do |s|
	s.name = %{#{name}}
	s.version = %{#{version}}
    s.author = "Kevin Williams"
    s.email = "kevin@bantamtech.com"
    s.homepage="http://mysql-win.rubyforge.org"
    s.summary = "A win32-native build of the MySQL API module for Ruby."
    s.description = s.summary
    s.rubyforge_project = s.name
    s.files += %w(ext/mysql.so ext/extconf.rb ext/extconf.rb.orig ext/mysql.c.in ext/test.rb README Rakefile docs)
	s.rdoc_options << '--exclude' << 'ext' << '--main' << 'README'
	s.extra_rdoc_files = ["README"]
	s.has_rdoc = true
	s.require_path = 'ext'
	s.autorequire = 'mysql'
    s.required_ruby_version = '>= 1.8.2'
    s.platform = Gem::Platform::WIN32
  end

  Gem::manage_gems
  Gem::Builder.new(spec).build
end

