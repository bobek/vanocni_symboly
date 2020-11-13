version_string = `git describe --tags`.chomp
if version_string.empty?
  version_string = '0'
end
date_string = Time.now.strftime("%Y-%m-%d")
source = "index.adoc"
params = "--attribute revnumber='#{version_string}' --attribute revdate='#{date_string}' -D 'out' -a imagesdir='images'"

namespace :book do
  namespace :build do
    desc 'build all versions of the book'
    task :all => [:html, :epub, :mobi, :pdf, :rename]

    desc 'build html version of the book'
    task :html do
      puts "Converting to HTML..."
      `bundle exec asciidoctor #{params} -a toc=left -a ebook-format=html #{source}`
      FileUtils.cp_r('images', 'out', verbose: true)
    end

    desc 'build epub version of the book'
    task :epub do
      puts "Converting to EPub..."
      `bundle exec asciidoctor-epub3 #{params} #{source}`
    end

    desc 'build mobi version of the book'
    task :mobi do
      puts "Converting to Mobi (kf8)..."
      `bundle exec asciidoctor-epub3 #{params} -a ebook-format=kf8 #{source}`
    end

    desc 'build pdf version of the book'
    task :pdf do
      puts "Converting to PDF... (this one takes a while)"
      `bundle exec asciidoctor-pdf --trace #{params}  -r './styles/pdf/pdf-extensions.rb' -a pdfmarks -a media=screen #{source}`
      # `bundle exec asciidoctor-pdf --trace #{params}  -a pdfmarks -a media=screen #{source}`
    end

    desc 'rename generated files to something nicer'
    task :rename do
      Dir.glob(File.join('out', 'index*')) do |f|
        new = f.gsub(/index/, 'vanocni_symboly').gsub(/-kf8\.epub/, '.mobi')
        puts("#{f} -> #{new}")
        File.rename(f, new)
      end
    end
  end
end

task :default => "book:build:all"
