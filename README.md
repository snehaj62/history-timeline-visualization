import React, { useState } from 'react';
import './App.css';
import IndianHistory from './IndianHistory';
import SportsHistory from './SportsHistory';
import GlobalHistory from './GlobalHistory1';
import WarsHistory from './WarsHistory1';
import CultureHistory from './CultureHistory';
import ScienceHistory from './ScienceHistory';
import EnvironmentHistory from './EnvironmentHistory';
import IndependenceHistory from './IndependenceHistory';

function App() {
  const [currentPage, setCurrentPage] = useState(null);
  const [showDropdown, setShowDropdown] = useState(false);
  const [showAddTopicForm, setShowAddTopicForm] = useState(false);
  const [userAddedTopics, setUserAddedTopics] = useState([]);
  const [userHistoryDropdown, setUserHistoryDropdown] = useState(false);
const [searchQuery, setSearchQuery] = useState('');
  const [searchResults, setSearchResults] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleSearch = async () => {
    setLoading(true);
    try {
      const response = await fetch(https://en.wikipedia.org/w/api.php?action=query&format=json&list=search&srsearch=${encodeURIComponent(searchQuery)}&origin=*);
      const data = await response.json();
      setSearchResults(data.query.search);
    } catch (error) {
      console.error('Error fetching search results:', error);
    }
    setLoading(false);
  };


  const toggleDropdown = () => setShowDropdown(!showDropdown);

  const handleClick = (page) => {
    setCurrentPage(page);
    setShowDropdown(false);
    setShowAddTopicForm(false);
    setUserHistoryDropdown(false); // Reset dropdown state
  };

  const toggleAddTopicForm = () => setShowAddTopicForm(!showAddTopicForm);

  const handleAddTopic = (event) => {
    event.preventDefault();
  const newTopicName = event.target.elements.topicName.value;
    const newContent = event.target.elements.topicContent.value;

    const newTopic = {
      name: newTopicName,
      content: [newContent],
    };

    setUserAddedTopics([...userAddedTopics, newTopic]);
    setShowAddTopicForm(false);
    event.target.reset(); // Reset the form
  };

  const handleGoBack = () => setCurrentPage(null);

  const toggleUserHistoryDropdown = () => setUserHistoryDropdown(!userHistoryDropdown);

  return (
    <div>
      <nav className="navbar">
        <h1 className="logo">Historical Timeline</h1>
        
        <div className="dropdown-container">
          <button className="dropdown-btn" onClick={toggleDropdown}>
            Histories
          </button>
          
          {showDropdown && (
            <ul className="dropdown-menu">
              <li><button className="dropdown-link" onClick={() => handleClick('indian')}>Indian History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('sports')}>Sports History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('global')}>Global History</button></li>
              <li><button class="dropdown-link" onClick={() => handleClick('wars')}>Wars History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('culture')}>Culture History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('science')}>Science History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('environment')}>Environmental History</button></li>
              <li><button className="dropdown-link" onClick={() => handleClick('independence')}>Independence History</button></li>
            </ul>
          )}
        </div>
        
      </nav>

      <div className="add-topic-form">
          <form onSubmit={handleAddTopic}>
            <input type="text" className='new' name="topicName" placeholder="New Topic Name" required /><br/>
            <input type="text" className='new' name="topicContent" placeholder="Initial Content" required /><br/>
            <button className='add-top' type="submit">Add Topic</button>
          </form>
        </div>
      <div className="container">
      
        {currentPage === null && (
          <div className="history-boxes">
            <div className="history-box" onClick={() => handleClick('indian')}>
              <h2>Indian History</h2>
              <p>India is a diverse and vibrant country located in South Asia. It\'s known for its rich history, cultural heritage, and bustling cities. From the majestic Himalayas in the north to the tropical beaches in the south, India offers a wide range of landscapes and experience With a population of over 1.3 billion people, </p>
            </div>
            <div className="history-box" onClick={() => handleClick('sports')}>
              <h2>Sports History</h2>
              <p>Iconic moments and champions It encompasses the development, evolution, and significant events of sports activities throughout human history. This periodization often divides sports history into key eras or epochs to better understand how sports have transformed culturally, socially. </p>
            </div>
            <div className="history-box" onClick={() => handleClick('global')}>
              <h2>Global History</h2>
              <p>Global history offers a panoramic view of human civilization, tracing the intricate tapestry of events, interactions, and transformations that have shaped societies across continents and epochs. From the emergence of early civilizations to the complexities of the modern world, the story of humanity unfolds </p>
            </div>
            <div className="history-box" onClick={() => handleClick('wars')}>
              <h2>Wars History</h2>
              <p>The history of warfare is a testament to humanit\'s capacity for both innovation and destruction, with conflicts shaping the course of nations, cultures, and civilizations throughout the ages. From ancient battles fought with spears and swords to modern conflicts waged with advanced technology and global alliances.</p>
            </div>
            <div className="history-box" onClick={() => handleClick('culture')}>
              <h2>Culture History</h2>
              <p>Origins of Human Culture: The history of human culture stretches back tens of thousands of years to the dawn of civilization, when early humans began to develop language, art, music, and social customs.</p>
            </div>
            <div className="history-box" onClick={() => handleClick('science')}>
              <h2>History of Science</h2>
              <p>The history of science traces its origins to ancient civilizations such as Mesopotamia, Egypt, Greece, and China, where scholars laid the groundwork for systematic observation, experimentation, and inquiry.</p>
            </div>
            <div className="history-box" onClick={() => handleClick('environment')}>
              <h2>Environmental History</h2>
              <p>Human impact on the environment, Throughout history, humans have interacted with the natural world in diverse ways, shaping and being shaped by their environments. From hunter-gatherer societies to early agricultural civilizations, ancient peoples developed a range of beliefs, practices, and technologies to adapt</p>
            </div>
            <div className="history-box" onClick={() => handleClick('independence')}>
              <h2>Search from wikipedia</h2>
              <p>Search directly</p>
            </div>

            {/* Other predefined history boxes */}
            
          </div>
        )}
      
        {currentPage !== null && (
          <div>
            <h2>{currentPage}</h2>
            {userAddedTopics.find((topic) => topic.name === currentPage)?.content.map((content, index) => (
              <p key={index}>{content}</p>
            ))}
<div className='newcon'>
            {userAddedTopics.some((topic) => topic.name === currentPage) && (
              <form onSubmit={(event) => {
                event.preventDefault();
                const newContent = event.target.elements.newContent.value;
                const updatedTopics = userAddedTopics.map((topic) => {
                  if (topic.name === currentPage) {
                    topic.content.push(newContent);
                  }
                  return topic;
                });

                setUserAddedTopics(updatedTopics);
                event.target.reset(); // Reset the form
              }}>
                <input type="text" name="newContent" placeholder="Add more content" required />
                <button type="submit">Add Content</button>
              </form>
            )}
            </div>
{currentPage === 'indian' && <IndianHistory />}
        {currentPage === 'sports' && <SportsHistory />}
        {currentPage === 'global' && <GlobalHistory />}
        {currentPage === 'wars' && <WarsHistory />}
        {currentPage === 'culture' && <CultureHistory />}
        {currentPage === 'science' && <ScienceHistory />}
        {currentPage === 'environment' && <EnvironmentHistory />}
        {currentPage === 'independence' && <IndependenceHistory />}
        
        
            <button className="go-back-button" onClick={handleGoBack}>
              Go Back
            </button>
          </div>
        )}
      </div>
 
        
      

        <div className="user-history-dropdown">
              <button onClick={toggleUserHistoryDropdown}>User-Added History Boxes</button>

              {userHistoryDropdown && (
                <ul className="user-history-dropdown-menu">
                  {userAddedTopics.map((topic, index) => (
                    <li key={index}>
                      <button onClick={() => handleClick(topic.name)}>{topic.name}</button>
                    </li>
                  ))}
                </ul>
              )}
            </div>
    </div>
  );
}

export default App;
