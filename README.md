# Lap_Trinh_Mang

## Lấy thông tin chi tiết IP
    
   ```csharp
    using System;
    using System.Net;
    using System.Net.NetworkInformation;
    using System.Diagnostics;

    public class IpInfo
    {
        public static void Main(string[] args)
        {
            try
            {
                // Lấy thông tin địa chỉ IP của local host
                IPAddress ipAddress = Dns.GetHostEntry(IPAddress.Parse("127.0.0.1")).AddressList[0];

                // Lấy subnet mask bằng cách sử dụng IPAddress.GetDefaultPrefixSuffix
                string subnetMask = ipAddress.GetDefaultPrefixSuffix().ToString();

                // Lấy default gateway bằng lệnh ipconfig
                string defaultGateway = GetDefaultGateway();

                Console.WriteLine("Thông tin giao thức IP của Local Host:");
                Console.WriteLine("----------------------------------");
                Console.WriteLine($"Địa chỉ IP: {ipAddress}");
                Console.WriteLine($"Subnet Mask: {subnetMask}");
                Console.WriteLine($"Default Gateway: {defaultGateway}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Đã xảy ra lỗi: {ex.Message}");
            }

            Console.ReadKey();
        }

        static string GetDefaultGateway()
        {
            try
            {
                Process process = new Process();
                process.StartInfo.FileName = "powershell.exe";
                process.StartInfo.Arguments = "-Command (Get-NetRoute -DestinationPrefix 0.0.0.0/0).NextHop"; //This may need tweaking.
                process.StartInfo.UseShellExecute = false;
                process.StartInfo.RedirectStandardOutput = true;
                process.StartInfo.RedirectStandardError = true;
                process.Start();

                string output = process.StandardOutput.ReadToEnd();
                string error = process.StandardError.ReadToEnd();

                process.WaitForExit();

                if (string.IsNullOrEmpty(output))
                {
                    Console.WriteLine("Không thể lấy thông tin default gateway từ ipconfig.");
                    return "Không tìm thấy";
                }

                Console.WriteLine("Output: " + output);
                return output.Trim(); // Remove leading/trailing whitespace
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Lỗi lấy thông tin default gateway: {ex.Message}");
                return "Không tìm thấy";
            }
        }
    }
```
