package application.service;

import java.time.LocalDate;
import java.util.ArrayList;

import application.model.Booking;
import application.model.CompanyPerson;
import application.model.Conference;
import application.model.ExtraService;
import application.model.Hotel;
import application.model.Participant;
import application.model.Trip;
import storage.Storage;



public class Service 
{
	
//------------------------------- Companion -------------------------------------
	public static void connectCompanionToTrip(Trip trip, Booking booking)
	{
		trip.addCompanion(booking.getCompanion());
		booking.addTrip(trip);
	} 

	
//------------------------------- Participants -------------------------------------
	public static Participant createParticiPant(String name, int age, String country, String city, int phonenumber)
	{
		Participant participant = new Participant(name, age, country, city, phonenumber);
		Storage.addParticipant(participant);
		return participant;
	}
	
	public static void deleteParticipant(Participant participant)
	{
		for(Booking booking: Storage.getBookings())
		{
			booking.setParticipant(null);
		}
	}
	
	public static ArrayList<Participant> getParticipant()
	{
		return Storage.getParticipants();
	}
	
	public static void updateParticipant(Participant participant, String name, int age, String country, String city, int phonenumber)
	{
		participant.setName(name);
		participant.setAge(age);
		participant.setCountry(country);
		participant.setCity(city);
		participant.setPhoneNumber(phonenumber);
	}
	
	public static void removeParticiPantFromBooking(Participant participant, Booking booking)
	{
		if(participant != null)
		{
			booking.setParticipant(null);
		}
	}

	public static CompanyPerson createCompanyPerson(String name, int age, String country, String city, int phonenumber, String companyName)
	{
		CompanyPerson companyParticipant = new CompanyPerson(name, age, country, city, phonenumber, companyName);
		return companyParticipant;

	}
	
	public static ArrayList<Booking> getBookings()
	{
		return Storage.getBookings();
	}
	
//------------------------------- Conference -------------------------------------
	
	public static Conference createConference(String name, String address, double conferenceCost)
	{
		Conference conference = new Conference(name, address, conferenceCost);
		Storage.addConference(conference);
		return conference;
	}
	
	public static ArrayList<Conference> getConferences()
	{
		return Storage.getConference();
	}
	
	public static void updateConference(Conference conference, String name, String address)
	{
		conference.setAdress(address);
		conference.setConferenceName(name);
	}
	
	public static void findConference(Participant participant)
	{
		for(int i = 0; i < Storage.getBookings().size(); i++)
		{
			participant.getBooking().get(i).getConference();
		}
	}
	
	public static void deleteConferene(Conference conference)
	{
		Storage.removeConference(conference);
	}
	
	public static void connectParticipantToBooking(Participant participant, Booking booking)
	{
		booking.setParticipant(participant);
		participant.addBooking(booking);
	
	}
	
//------------------------------- Hotels -------------------------------------	
	
	public static Hotel createHotel(String hotelName, double twoPersonPrice, double onePersonPrice)
	{
		Hotel hotel = new Hotel(hotelName, twoPersonPrice, onePersonPrice);
		Storage.addHotel(hotel);
		return hotel;
	}
	
	public static ArrayList<Hotel> getHotels()
	{
		return Storage.getHotels();
	}
	
	public static ArrayList<ExtraService> getServices()
	{
		return Storage.getServices();
	}
	
	public static void connectParticipantToHotel(Hotel hotel, Participant participant)
	{
		Hotel oldHotel = participant.getHotel();
		if(oldHotel != null)
		{
			oldHotel.removeParticipant(participant);
		}
		participant.setHotel(hotel);
		hotel.addParticipant(participant);
		
	}
	
	public static void removeParticipantFromHotel(Participant participant)
	{
		Hotel hotel = participant.getHotel();
		if(hotel != null)
		{
			hotel.removeParticipant(participant);
			participant.setHotel(null);
		}
		
	}
	
// ---------------------- initStorage -----	
	public static void initStorage()
	{
		Participant malene = Service.createParticiPant("Malene Tran", 20, "Denmark", "Viby J, Aarhus", 42201100);
		Conference conference1 = Service.createConference("Hav og himmel", "Fyrrestien 2", 100);
		Conference conference2 = Service.createConference("Elektronik og spas", "Fyrrestien 2", 200);
		Booking booking = conference1.createBooking(false, LocalDate.of(1995, 11, 12), LocalDate.of(1995, 11, 20), conference1, null, null);
		Booking booking2 = conference2.createBooking(false, LocalDate.of(1995, 11, 12), LocalDate.of(1995, 11, 20), conference2, null, null);
		Service.connectParticipantToBooking(malene, booking2);
		Service.connectParticipantToBooking(malene, booking);
		
		Participant michael = Service.createParticiPant("Michael Andersen", 20, "Denmark", "Viby J, Aarhus", 42201100);
		Booking bookings = conference1.createBooking(false, LocalDate.of(1997, 11, 12), LocalDate.of(1997, 11, 20), conference1, null, null);
		Service.connectParticipantToBooking(michael, bookings);
		
		Hotel hilton = Service.createHotel("Hilton Hotel", 200, 200);
		hilton.createService(100, "wifi");
		
		Hotel maiden = Service.createHotel("Maiden Hotel", 200, 200);
		maiden.createService(100, "andet");
		maiden.createService(200, "write me a song");
		
		Service.connectParticipantToHotel(hilton, michael);
		conference1.addHotel(hilton);
		conference1.addHotel(maiden);
		conference2.addHotel(hilton);
		
	}
	

}
