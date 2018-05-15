require 'rake'
require 'yaml'
require 'erb'
require 'tilt'
require 'fileutils'
require 'english'

DOCKERHUB_ORGANIZATION = 'dkdde'.freeze
CONTAINER_NAME = 'jenkins-worker'.freeze
DOCKER_TEMPLATE = 'Dockerfile.template.erb'.freeze
YAML_FILE = 'configurations.yml'.freeze
LINE_SEPARATOR = ('=' * 80).freeze

desc 'start the whole compilcation process'
task :default do
  Rake::Task['build'].invoke
  Rake::Task['compile'].invoke
end

desc 'create Dockerfiles'
task :build do
  data = YAML.load_file("#{__dir__}/#{YAML_FILE}")
  data.each do |key, values|
    FileUtils.mkdir_p key

    template = Tilt.new(File.join(__dir__, DOCKER_TEMPLATE))
    dockerfile_content = template.render nil, values

    File.write(File.join(__dir__, key, 'Dockerfile'), dockerfile_content)
  end
end

desc 'compile Dockerfiles to Docker images'
task :compile do
  data = YAML.load_file("#{__dir__}/#{YAML_FILE}")
  data.each_key do |key|
    puts LINE_SEPARATOR
    puts "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
    puts "docker build -t #{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key} #{key} --compress --squash"
    puts LINE_SEPARATOR
    system("docker build -t #{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key} #{key} --compress --squash")
    raise('Exit due to error!') unless $CHILD_STATUS.success?
  end
end
