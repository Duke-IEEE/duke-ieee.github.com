require 'yaml'

ROOT_DIR = File.dirname(__FILE__)
CONFIG_FILE = File.join(ROOT_DIR, '_config.yml')
CONFIG = YAML.load_file(CONFIG_FILE)
DEST_DIR = File.join(ROOT_DIR, CONFIG['destination'] || '_site')
COMPRESSOR = File.join(ROOT_DIR, 'yuicompressor-2.4.7.jar')


desc "Build site using jekyll and minify assets."
task :build do
  system "jekyll --no-auto"
  minify_css
  minify_js
end

desc "Deploy the compiled version of the site using git."
task :deploy do
  original_branch = git_branch
  system "git checkout master"

  files = Dir.glob(File.join(DEST_DIR, "*"))
  FileUtils.mv files, ROOT_DIR, verbose: true

  system "git add #{ROOT_DIR}"
  system "git commit -m \"#{deploy_commit_message}\""
  system "git push origin master"
  system "git checkout #{original_branch}"
end


# Helper methods

def minify_css
  css_file = File.join(DEST_DIR, 'css', 'main.css')
  system "java -jar #{COMPRESSOR} #{css_file} -o #{css_file}"
end

def minify_js
  js_file = File.join(DEST_DIR, 'js', 'main.js')
  system "java -jar #{COMPRESSOR} #{js_file} -o #{js_file}"
end

def deploy_commit_message
  "Deployed at #{Time.now}"
end

def git_branch
  `git branch` =~ /\* (.*)$/
  $1
end
