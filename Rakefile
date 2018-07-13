require 'rake'
require 'yaml'
require 'erubi'
require 'tilt'
require 'fileutils'

DOCKERHUB_ORGANIZATION = 'dkdde'.freeze
CONTAINER_NAME = 'jenkins-worker'.freeze
DOCKER_TEMPLATE = 'Dockerfile.template.erb'.freeze
YAML_FILE = 'configurations.yml'.freeze

desc 'start the whole compilcation process'
task :default do
  Rake::Task['build'].invoke
  Rake::Task['compile'].invoke
end

desc 'create Dockerfiles'
task :build do
  data = YAML.load_file(File.join(__dir__, YAML_FILE))
  data.each do |key, values|
    FileUtils.mkdir_p key

    variables = values.merge dockerhub_organisation: DOCKERHUB_ORGANIZATION, container_name: CONTAINER_NAME, container_tag: key
    template = Tilt.new(File.join(__dir__, DOCKER_TEMPLATE))
    dockerfile_content = template.render nil, variables

    File.write(File.join(__dir__, key, 'Dockerfile'), dockerfile_content)
  end
end

desc 'compile Dockerfiles to Docker images'
task :compile do
  data = YAML.load_file(File.join(__dir__, YAML_FILE))
  data.each_key do |key|
    container_name = "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
    raise('Premature exit due to error!') unless system("docker build -t #{container_name} #{key} --compress --squash")
  end
end

desc 'show Docker commmands to build images'
task :dryrun do
  data = YAML.load_file(File.join(__dir__, YAML_FILE))
  data.each_key do |key|
    container_name = "#{DOCKERHUB_ORGANIZATION}/#{CONTAINER_NAME}:#{key}"
    puts
    puts "Container: #{container_name}"
    puts "  docker build -t #{container_name} #{key} --compress --squash"
    puts "  docker push #{container_name}"
  end
end
