USER = "div"
REPO = "div.github.com"
desc "deploy build directory to github pages"
task :deploy do
  puts "## Compiling ruhoh "
  system "ruhoh compile"
  puts "## Deploying branch to Github Pages "
  cd "compiled" do
    system "git init ."
    File.new(".nojekyll", "w").close
    cname = File.new("CNAME", "w")
    cname << "www.divrb.com"
    cname.close
    system "git add ."
    system "git add -u"
    puts "\n## Commiting: Site updated at #{Time.now.utc}"
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m \"#{message}\""
    puts "\n## Pushing generated website"
    system "git push git@github.com:#{USER}/#{REPO} master:master --force"
    puts "\n## Github Pages deploy complete"
    system "rm -rf .git"
  end
end