# frozen_string_literal: true

require 'rake'
require 'yaml'
require 'erubi'
require 'tilt'
require 'fileutils'

DOCKERHUB_ORGANIZATION = 'dkdde'
CONTAINER_NAME = 'jenkins-worker'
DOCKER_TEMPLATE = 'Dockerfile.template.erb'
YAML_FILE = 'configurations.yml'

def load_configurations
  file_content = YAML.load_file(File.join(__dir__, YAML_FILE))
  file_content.reject { |key| key.start_with? '_' }
end

def container_name(key)
  "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
end

desc 'start the whole compilcation process'
task :default do
  Rake::Task['build'].invoke
  Rake::Task['compile'].invoke
end

desc 'create Dockerfiles'
task :build do
  load_configurations.each do |key, values|
    puts
    puts "Container: #{container_name(key)}"
    FileUtils.mkdir_p key
    variables = values.merge dockerhub_organisation: DOCKERHUB_ORGANIZATION, container_name: CONTAINER_NAME, container_tag: key
    template = Tilt.new(File.join(__dir__, DOCKER_TEMPLATE))
    dockerfile_content = template.render nil, variables
    File.write(File.join(__dir__, key, 'Dockerfile'), dockerfile_content)
  end
end

desc 'compile Dockerfiles to Docker images'
task :compile do
  load_configurations.each_key do |key|
    puts
    puts "Container: #{container_name(key)}"
    raise('Premature exit due to error!') unless system("docker build -t #{container_name(key)} #{key}") # --compress --squash
  end
end

desc 'show Docker commmands to build images'
task :dryrun do
  load_configurations.each_key do |key|
    container_name = "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
    puts
    puts "Container: #{container_name(key)}"
    puts "  docker build -t #{container_name} #{key}" # --compress --squash
    puts "  docker push #{container_name}"
  end
end

desc 'upload images to DockerHub'
task :upload do
  load_configurations.each_key do |key|
    container_name = "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
    puts
    puts "Container: #{container_name(key)}"
    system "docker push #{container_name}"
  end
end
