require "stringex"

ssh_user       = ""
ssh_port       = ""
document_root  = "~/html/"
rsync_delete   = true
deploy_default = "rsync"
rsync_args     = "" 

## -- Misc Configs -- ##

public_dir      = "_site"    # compiled site directory
posts_dir       = "_drafts"
source_dir      = "."    # source file directory
blog_index_dir  = '.'    # directory for your blog's index page (if you put your index in source/blog/index.html, set this to 'source/blog')
server_port     = "4000"  
new_post_ext    = "markdown"  # default new post file extension when using the new_post task

desc "Deploy website via rsync"
task :deploy do
  exclude = ""
  if File.exists?('./rsync-exclude')
    exclude = "--exclude-from '#{File.expand_path('./rsync-exclude')}'"
  end
  puts "## Deploying website via Rsync"
  system("rsync -avze 'ssh -p #{ssh_port}' #{exclude} #{rsync_args} #{"--delete" unless rsync_delete == false} #{public_dir}/ #{ssh_user}:#{document_root}")
end

desc "preview the site in a web browser"
task :preview do
  system 'jekyll serve'
end

desc 'generates the site'
task :generate do
  system 'jekyll build'
end

desc "a new post in #{source_dir}/#{posts_dir}"
task :new_post, :title do |t, args|
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  mkdir_p "#{source_dir}/#{posts_dir}"
  filename = "#{source_dir}/#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "author: "
    post.puts "---"
  end
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end