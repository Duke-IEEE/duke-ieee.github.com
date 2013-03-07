ROOT_DIR = File.dirname(__FILE__)
DEST_DIR = File.join(ROOT_DIR, "_site")


desc "Deploy the compiled version of the site using git."
task :deploy do
  original_branch = git_branch
  system "jekyll --no-auto"
  system "git checkout master"

  files = Dir.glob(File.join(DEST_DIR, "*"))
  FileUtils.mv files, ROOT_DIR, verbose: true

  system "git add #{ROOT_DIR}"
  system "git commit -m \"#{deploy_commit_message}\""
  system "git push origin master"
  system "git checkout #{original_branch}"
end

def deploy_commit_message
  "Deployed at #{Time.now}"
end

def git_branch
  `git branch` =~ /\* (.*)$/
  $1
end
