# Groovy

## Tips

### ping

~~~
def proc = 'ping -c 3 192.168.1.100'.execute()
proc.waitFor()
println "Ping ${proc.exitValue() == 0 ? 'OK' : 'NG'}"
~~~

