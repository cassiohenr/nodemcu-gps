#include <arduino.h>
#include <SoftwareSerial.h>
#include <TinyGPS.h>

TinyGPS gps;
SoftwareSerial serGPS(14, 12, false, 256);     //14 é D5 ;  12 é D6

static void smartdelay(unsigned long ms);
byte month, day, hour, minute, second, hundredths;
int year;
unsigned long age;

NMEA nmeaDecoder(ALL);

void setup()
{
  Serial.begin(19200);
  serGPS.begin(9600);
  dht.begin();
}

void loop()
{
  float flat, flon;
  gps.f_get_position(&flat, &flon, &age);
  Serial.print("Latitude: "); Serial.println(flat,6);
  Serial.print("Longitude: "); Serial.println(flon,6);
  Serial.print("Satelites: "); Serial.println(gps.satellites());
  Serial.print("HDOP: "); Serial.print(gps.hdop()/100);
  Serial.print(gps.satellites()); Serial.print(" ");
  Serial.print("Velocidade: "); Serial.println(gps.f_speed_kmph());

  Serial.print("Time: ");
  gps.crack_datetime(&year, &month, &day, &hour, &minute, &second, &hundredths, &age);
  char sz[32];
  sprintf(sz, "%02d/%02d/%02d %02d:%02d:%02d ", month, day, year, hour, minute, second);
  Serial.println(sz);

  smartdelay(1000);
}

static void smartdelay(unsigned long ms)
{
  unsigned long start = millis();
  do
  {
    while (serGPS.available())
      gps.encode(serGPS.read());
  } while (millis() - start < ms);
}
