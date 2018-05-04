# Htmlpdfに直リンクする（ダウンロードしない）方法


Add to app/models/attachment.rb, somewhere next to def thumbnailable:


~~~
def pdf?
!!(self.filename =~ /.(pdf)$/i)
end
~~~

Then modify app/controllers/attachments_controller.rb:

Change the line:


:disposition => (@attachment.image? ? 'inline' : 'attachment')


to


:disposition => (@attachment.image? || @attachment.pdf? ? 'inline' : 'attachment')

