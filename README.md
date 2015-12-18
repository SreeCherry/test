import java.io.BufferedReader;
import java.io.FileReader;
import java.io.FileWriter;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.sax.SAXTransformerFactory;
import javax.xml.transform.sax.TransformerHandler;
import javax.xml.transform.stream.StreamResult;

import org.xml.sax.SAXException;
import org.xml.sax.helpers.AttributesImpl;

/* 
 * This program reads a 4 lines text file land creates an XML structure around
 * the raw data. The XML structure is then copied to an xml output file. 
 */
public class Test {

    BufferedReader in;
    StreamResult out;

    TransformerHandler th;
    AttributesImpl atts;

    // Here's our entry point ... 
    public static void main(String args[]) {
        new Test().doit();
    }

    public void doit() {
        try {
            in = new BufferedReader(new FileReader("D:\\abc.txt"));
            out = new StreamResult(new FileWriter("D:\\abc.xml"));
            initXML();
            String str;
            // declare an array that can contain 4 strings
            String[] SArray=new String[4];
            String s;
            int i = 0;
            while ((s = in.readLine()) != null) {
            	String a[]=s.split("<br />");
            	for(int j=0;j<a.length;j++)
            	{
            		System.out.println(a[j]);
            	}
            	 process(a);
				System.out.println(s);
			}

            while ((str = in.readLine()) != null) {
//              System.out.println("input line: " + str);
                SArray[i]=str;
                i++;

            }

           

            in.close();
            closeXML();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void initXML() throws ParserConfigurationException,
            TransformerConfigurationException, SAXException {
        // JAXP + SAX
        SAXTransformerFactory tf = (SAXTransformerFactory) SAXTransformerFactory
                .newInstance();

        th = tf.newTransformerHandler();
        Transformer serializer = th.getTransformer();
        serializer.setOutputProperty(OutputKeys.ENCODING, "ISO-8859-1");
        // pretty XML output
        serializer.setOutputProperty(
                "{http://xml.apache.org/xslt}indent-amount", "4");
        serializer.setOutputProperty(OutputKeys.INDENT, "yes");
        th.setResult(out);
        th.startDocument();
        atts = new AttributesImpl();
        th.startElement("", "", "item-info", atts);

    }

    public void process(String[] a) throws SAXException {
//      System.out.println("number of elements: " + elements.length);
        atts.clear();

        th.startElement("", "", "jid", atts);
        th.characters(a[0].toCharArray(), 0, a[0].length());
        th.endElement("", "", "jid");

        th.startElement("", "", "aid", atts);
        th.characters(a[1].toCharArray(), 0, a[1].length());
        th.endElement("", "", "aid");

        th.startElement("", "", "amma", atts);
        th.characters(a[2].toCharArray(), 0, a[2].length());
        th.endElement("", "", "amma");

        th.startElement("", "", "anna", atts);
        th.characters(a[3].toCharArray(), 0, a[3].length());
        th.endElement("", "", "anna");

    }

    public void closeXML() throws SAXException {
        
        th.endElement("", "", "item-info");
        th.endDocument();
    }
}
