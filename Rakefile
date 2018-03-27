require 'rake'
require 'yaml'
require 'erb'
require 'fileutils'

DOCKER_TEMPLATE = 'Dockerfile.template'.freeze
YAML_FILE = 'template.yml'.freeze

desc 'create Dockerfiles'
task :build, :debug do |_, args|
  data = YAML.load_file("#{__dir__}/#{YAML_FILE}")
  data.each do |key, value|
    FileUtils.mkdir_p key
    @data = value
    @data['debug'] = true if args.debug
    erb = ERB.new(File.open("#{__dir__}/#{DOCKER_TEMPLATE}").read, 0, '>')
    File.open("#{__dir__}/#{key}/Dockerfile", 'w') do |file|
      file.write erb.result binding
    end
    FileUtils.cp_r 'sshd', "#{__dir__}/#{key}/"
  end
end

desc 'compile Dockerfiles to images'
task :compile do
  data = YAML.load_file("#{__dir__}/#{YAML_FILE}")
  data.each do |key, _|
    puts '=' * 80, key, "docker build -t dkdde/docker-jenkins-slave:#{key} #{key} --compress --squash", '=' * 80
    system("docker build -t dkdde/docker-jenkins-slave:#{key} #{key} --compress --squash")
    raise("Exit due to error!") unless $?.success?
  end
end

task :cleanup do
  data = YAML.load_file("#{__dir__}/#{YAML_FILE}")
  data.each do |key, _|
    FileUtils.rm_rf "#{__dir__}/#{key}/sshd"
  end
end

desc 'start the whole show'
task :default, :debug do |_, args|
  Rake::Task['cleanup'].invoke
  args.debug ? Rake::Task['build'].invoke(true) : Rake::Task['build'].invoke
  Rake::Task['compile'].invoke
end
