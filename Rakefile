namespace :cloud_speech_to_text do
  desc 'Encode MOV file to FLAC'
  task :encode do |t|
    mov = 'input/input.MOV'
    flac = 'output/output.flac'

    system 'flac', '-0', mov, '-f', '-o', flac, '--endian=big', '--sign=signed', '--channels=1', '--bps=16', '--sample-rate=16000'

  end

  desc 'Upload FLAC to GCS'
  task :upload do |t|
  end

  desc 'request cloud speech_to_text api'
  task :request do |t|
  end
end

