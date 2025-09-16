```csharp
using System;
using System.Net;
using System.Net.NetworkInformation;

class Program
{
    static void Main()
    {
        // Lấy thông tin về các giao diện mạng trên máy
        NetworkInterface[] networkInterfaces = NetworkInterface.GetAllNetworkInterfaces();

        foreach (NetworkInterface netInterface in networkInterfaces)
        {
            // Kiểm tra xem giao diện mạng có khả dụng không
            if (netInterface.OperationalStatus == OperationalStatus.Up)
            {
                // Lấy thông tin cấu hình IP cho giao diện mạng này
                IPInterfaceProperties ipProperties = netInterface.GetIPProperties();

                // Lấy địa chỉ IPv4 (chỉ lấy IP IPv4)
                foreach (UnicastIPAddressInformation ipAddress in ipProperties.UnicastAddresses)
                {
                    if (ipAddress.Address.AddressFamily == System.Net.Sockets.AddressFamily.InterNetwork)
                    {
                        Console.WriteLine("Interface: " + netInterface.Name);
                        Console.WriteLine("IP Address: " + ipAddress.Address);
                        Console.WriteLine("Subnet Mask: " + ipAddress.IPv4Mask);
                    }
                }

                // Lấy Default Gateway
                foreach (GatewayIPAddressInformation gateway in ipProperties.GatewayAddresses)
                {
                    Console.WriteLine("Default Gateway: " + gateway.Address);
                }

                Console.WriteLine(new string('-', 30));
            }
        }

        Console.ReadLine();
    }
}
```
