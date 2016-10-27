Just run: `docker-compose up -d` to start Graylog server with basic http security on GELF endpoint.

### Sending in log data

```bash
curl --user admin:admin -XPOST http://localhost:3000/gelf -p0 -d '{"short_message":"Hello there", "host":"example.org", "facility":"test", "_foo":"bar"}' 
```