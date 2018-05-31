Appendix E: Example of Python Driver
************************************

#Search pyhton device 

| def find_device(self)
|     while not self.autobaud(self.find_ports())
|         time.sleep(2)

#Search device's port

| def find_ports(self) 
|        print('scanning ports')
        
|        if sys.platform.startswith('win')
|            ports = ['COM%s' % (i + 1) for i in range(256)]
|        elif sys.platform.startswith('linux') or sys.platform.startswith('cygwin')
|            # this excludes your current terminal "/dev/tty"
|            ports = glob.glob('/dev/tty[A-Za-z]*')
|        elif sys.platform.startswith('darwin')
|            ports = glob.glob('/dev/tty.*')
|        else:
|            raise EnvironmentError('Unsupported platform')

|        result = []
|        for port in ports:
|            try:
|                print('Trying: ' + port)
|                s = serial.Serial(port)
|                s.close()
|                result.append(port)
|            except (OSError, serial.SerialException):
|                pass
|        return result

#automatically choose baud rate 

| def autobaud(self, ports):
|        for port in ports:
|            for baud in [115200, 57600, 38400, 19200, 9600]:
|                self.ser = serial.Serial(port, baud, timeout = 0.1)
|                # sync() works for stream mode
|                self.sync()
                
|                if self.stream_mode:
|                    print('Connected Stream Mode ' + '{0:d}'.format(baud) + '  ' + port)
|                    break
|                else:
|                    self.ser.close()
            
|            # stream mode not found for port, check port by polling
            
|            if self.stream_mode == 0:  
|                for baud in [115200, 57600, 38400, 19200, 9600]:
|                    self.ser = serial.Serial(port, baud, timeout = 0.1)
|                    self.device_id = self.get_id_str()
|                    if self.device_id:
|                        print('Connected Polled Mode ' + '{0:d}'.format(baud))
|                        # return it to streamed mode and sync
|                        self.odr_setting = 0x01
|                        self.set_fields([[0x0001, self.odr_setting]])
|                        self.sync()     
|                        print('Now Connectd Stream Mode ' + '{0:d}'.format(baud)) 
|                        return True               
|                    else:
|                        self.ser.close()

|            # in stream stream mode worked, get odr field and id str
|            else:
|                odr = self.read_fields([0x0001])
|                if odr:
|                    print(odr)
|                    # self.odr_setting = sum(odr[3:5])    # read ODR field
|                    # TODO: this is an issue, not getting odr field view
|                    self.odr_setting = 0x0001
|                    self.device_id = self.get_id_str()  # read device string
|                    self.restore_odr()
|                    return True
|                else:
|                    print('failed to get id string')
|                    return False
        
|        return False

#Connect device 

| def connect(self):
|        self.connected = 1
|        while self.connected:
|            #self.get_bit_status()
|            self.get_packet()
|        return  # ends thread

#disconnect device

| def disconnect(self):
|        self.connected = 0

| #Sync with device
| def sync(self,prev_byte = 0,bytes_read = 0):
|        try:    
|            S = bytearray(self.ser.read(1))
|        except serial.SerialException:
|            S = []
|            print('serial exception')
|            self.find_device()

|        if not len(S):
|            return False
|        if S[0] == 85 and prev_byte == 85:      # VALID HEADER FOUND
|            # Once header is found then read off the rest of packet
|            self.stream_mode = 1
|            config_bytes = bytearray(self.ser.read(3))
|            self.packet_type = '{0:1c}'.format(config_bytes[0]) + '{0:1c}'.format(config_bytes[1])
|            self.packet_size = config_bytes[2]
|            bytearray(self.ser.read(config_bytes[2] + 2))      # clear bytes off port, payload + 2 byte CRC
|            return True
|        else: 
|            # Repeat sync to search next byte pair for header
|            if bytes_read == 0:
|                print('Connecting ....')
|            bytes_read = bytes_read + 1
|            self.stream_mode = 0
|            if (bytes_read < 40):
|                self.sync(S[0], bytes_read)
|            else:
|                return False


