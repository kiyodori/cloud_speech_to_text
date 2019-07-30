namespace :cloud_speech_to_text do
  desc 'Encode MOV file to FLAC'
  task :encode do |t|
    mov = 'input/input.MOV'
    flac = 'output/output.flac'

    system 'ffmpeg', '-i', mov, '-ac', '1', '-c:a', 'flac', flac

  end

  desc 'Upload FLAC to GCS'
  task :upload do |t|
    flac = 'output.flac'
    system 'gsutil', 'cp', '-a', 'public-read', "output/#{flac}", 'gs://cloud_speech_to_text/'
  end

  desc 'request cloud speech_to_text api'
  task :request do |t|
    SPEAKER_COUNT = 2

    flac = 'output/output.flac'
    rate = `soxi -r #{flac}`.chomp.to_i

    result = request_api(rate, SPEAKER_COUNT)
    open('output/api_result.txt', 'a') do |f|
      f.puts result
    end
  end

  def request_api(rate, speaker_count)
    require 'json'
    require 'net/http'
    require 'dotenv'

    Dotenv.load

    flac = 'output.flac'
    uri = URI.parse "https://speech.googleapis.com/v1p1beta1/speech:recognize?key=#{ENV['GCLOUD_API_KEY']}"

    body = {
      config: {
        encoding: "FLAC",
        sampleRateHertz: rate,
        languageCode: "ja-JP",
        enableSpeakerDiarization: true,
        diarizationSpeakerCount: speaker_count
      },
      audio: {
        uri: "gs://cloud_speech_to_text/#{flac}"
      }
    }

    http = Net::HTTP.new(uri.host, uri.port)
    http.use_ssl = true

    req = Net::HTTP::Post.new uri.request_uri
    req['Content-Type'] = 'application/json'
    req.body = JSON.dump body

    resp = http.request req

    JSON.parse resp.body
  end
end

