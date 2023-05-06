using System;
using System.IO;
using System.Net;
namespace emailsender
{
    class Program
    {
        static void Main(string[] args)
        {
            foreach (string cmdline in args)
            {
                string[] inputdata = cmdline.Split(new string[] { "{||}" }, StringSplitOptions.None);

                string smtpserver = inputdata[0].ToString();
                int smtpport =int.Parse(inputdata[1].ToString());
                Boolean SSLEnable =Boolean.Parse(inputdata[2].ToString());
                string Fromaddress = inputdata[3].ToString();
                string Frompassword = inputdata[4].ToString();
                string toaddress = inputdata[5].ToString();
                string CCaddress = inputdata[6].ToString();
                string BCaddress = inputdata[7].ToString();
                string mailsubject = inputdata[8].ToString();
                Boolean HTMLBody = Boolean.Parse(inputdata[9].ToString());
                string mailbody = inputdata[10].ToString();
                string mailattachment= inputdata[11].ToString();
                Console.WriteLine("Sending Mail");
                SendMail(smtpserver, smtpport, SSLEnable, Fromaddress, Frompassword, toaddress, CCaddress, BCaddress, mailsubject, HTMLBody, mailbody, mailattachment);
            }
        }

        public static bool SendMail(string smtpAddress, int smtpPort, bool isSSLEnabled, string fromAddress, string fromPassword, string toMultipleAddressesSeperatedByComma, string ccMultipleAddressesSeperatedByComma, string bccMultipleAddressesSeperatedByComma, string mailSubject, bool isHTMLBody, string mailBody, string listOfAttachments)
        {

            bool returnValue = true;
            try
            {
                System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
                System.Net.NetworkCredential networkCredential = new System.Net.NetworkCredential(fromAddress, fromPassword);
                System.Net.Mail.MailMessage mailMessage = new System.Net.Mail.MailMessage();
                string[] toMultipleAddresses = toMultipleAddressesSeperatedByComma.Split(',');
                string[] ccMultipleAddresses = ccMultipleAddressesSeperatedByComma.Split(',');
                string[] bccMultipleAddresses = bccMultipleAddressesSeperatedByComma.Split(',');

                #region To Address
                if (toMultipleAddresses != null & toMultipleAddresses.Length > 0)
                {
                    foreach (string s in toMultipleAddresses)
                    {
                        if (s != null && s.Trim().Length > 0)
                        {
                            mailMessage.To.Add(new System.Net.Mail.MailAddress(s));
                        }
                    }
                }
                #endregion

                #region CC
                if (ccMultipleAddresses != null & ccMultipleAddresses.Length > 0)
                {
                    foreach (string s in ccMultipleAddresses)
                    {
                        if (s != null && s.Trim().Length > 0)
                        {
                            mailMessage.CC.Add(new System.Net.Mail.MailAddress(s));
                        }
                    }
                }
                #endregion

                #region BCC
                if (bccMultipleAddresses != null & bccMultipleAddresses.Length > 0)
                {
                    foreach (string s in bccMultipleAddresses)
                    {
                        if (s != null && s.Trim().Length > 0)
                        {
                            mailMessage.Bcc.Add(new System.Net.Mail.MailAddress(s));
                        }
                    }
                }
                #endregion

                #region Attachments
                if (listOfAttachments != "")
                {
                   

                    System.Net.Mail.Attachment attachment = new System.Net.Mail.Attachment(listOfAttachments, System.Net.Mime.MediaTypeNames.Application.Octet);
                    System.Net.Mime.ContentDisposition disposition = attachment.ContentDisposition;
                    disposition.CreationDate = File.GetCreationTime(listOfAttachments);
                    disposition.ModificationDate = File.GetLastWriteTime(listOfAttachments);
                    disposition.ReadDate = File.GetLastAccessTime(listOfAttachments);
                    disposition.FileName = Path.GetFileName(listOfAttachments);
                    disposition.Size = new FileInfo(listOfAttachments).Length;
                    disposition.DispositionType = System.Net.Mime.DispositionTypeNames.Attachment;
                    mailMessage.Attachments.Add(attachment);
                }
                #endregion

                mailMessage.From = new System.Net.Mail.MailAddress(fromAddress);
                mailMessage.Subject = mailSubject;
                mailMessage.Body = mailBody;
                mailMessage.IsBodyHtml = isHTMLBody;
                System.Net.Mail.SmtpClient smtpClient = new System.Net.Mail.SmtpClient(smtpAddress, smtpPort);
                smtpClient.UseDefaultCredentials = false;
                smtpClient.Credentials = networkCredential;
                smtpClient.DeliveryMethod = System.Net.Mail.SmtpDeliveryMethod.Network;
                smtpClient.EnableSsl = isSSLEnabled;
                smtpClient.Send(mailMessage);
            }
            catch (Exception ex)
            {
                returnValue = false;

                Console.WriteLine(ex.ToString());
            }

            return returnValue;
        }
    }
}
