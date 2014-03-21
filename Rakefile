Dir['./lib/tasks/**/*.rake'].each { |f| load f }

require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec

desc 'Push to staging.helabs.com.br'
task :staging do
  p '=> Memorizing current branch name...'
  current_branch = `git branch | grep "*"`.gsub('*', '').strip

  p '=> Remove staging branch...'
  system 'git branch -D staging'

  p '=> Create orphan staging branch...'
  system 'git checkout --orphan staging'

  p '=> Disallow robots...'
  File.open('robots.txt', 'w') { |file| file.write "User-agent: *\nDisallow: /" }

  p '=> Change CNAME...'
  File.open('CNAME', 'w') { |file| file.write 'staging.helabs.com.br' }

  p '=> Add everything...'
  system 'git add --all'

  p '=> Commit everything...'
  system 'git commit -m "Staging commit"'

  p '=> Add staging remote...'
  system 'git remote add staging git@github.com:Helabs/staging.helabs.com.br.git'

  p '=> Force push to staging. Get some coffee, it may take some time...'
  system 'git push -f staging staging:gh-pages'

  p "=> Checkout #{current_branch} branch..."
  system "git checkout #{current_branch}"

  p '=> Done. It can take up to 10 minutes for your changes to appear staging.helabs.com.br'
end
