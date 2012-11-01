USER = "div"
REPO = "blog"
desc "deploy build directory to github pages"
task :deploy do
  puts "## Compiling ruhoh "
  system "ruhoh compile"
  puts "## Deploying branch to Github Pages "
  cp_r ".nojekyll", "build/.nojekyll"
  cd "compiled/#{REPO}" do
    system "git init ."
    system "git add ."
    system "git add -u"
    puts "\n## Commiting: Site updated at #{Time.now.utc}"
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m \"#{message}\""
    puts "\n## Pushing generated website"
    system "git push git@github.com:#{USER}/#{REPO} master:gh-pages --force"
    puts "\n## Github Pages deploy complete"
    system "rm -rf .git"
  end
end