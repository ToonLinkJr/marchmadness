import React, { useState, useEffect } from 'react';
import { ChevronRight, ChevronLeft, Trophy } from 'lucide-react';

const MarchMadnessBracket = () => {
  // Teams for 2025 NCAA Tournament (placeholder data)
  const initialTeams = [
    // East Region
    { id: 1, name: 'Connecticut', seed: 1, region: 'East' },
    { id: 2, name: 'Iowa State', seed: 2, region: 'East' },
    { id: 3, name: 'Illinois', seed: 3, region: 'East' },
    { id: 4, name: 'Auburn', seed: 4, region: 'East' },
    { id: 5, name: 'San Diego State', seed: 5, region: 'East' },
    { id: 6, name: 'BYU', seed: 6, region: 'East' },
    { id: 7, name: 'Washington State', seed: 7, region: 'East' },
    { id: 8, name: 'FAU', seed: 8, region: 'East' },
    { id: 9, name: 'Northwestern', seed: 9, region: 'East' },
    { id: 10, name: 'Drake', seed: 10, region: 'East' },
    { id: 11, name: 'NC State', seed: 11, region: 'East' },
    { id: 12, name: 'UAB', seed: 12, region: 'East' },
    { id: 13, name: 'Yale', seed: 13, region: 'East' },
    { id: 14, name: 'Morehead State', seed: 14, region: 'East' },
    { id: 15, name: 'South Dakota State', seed: 15, region: 'East' },
    { id: 16, name: 'Stetson', seed: 16, region: 'East' },
    
    // West Region
    { id: 17, name: 'North Carolina', seed: 1, region: 'West' },
    { id: 18, name: 'Arizona', seed: 2, region: 'West' },
    { id: 19, name: 'Baylor', seed: 3, region: 'West' },
    { id: 20, name: 'Alabama', seed: 4, region: 'West' },
    { id: 21, name: 'Saint Mary\'s', seed: 5, region: 'West' },
    { id: 22, name: 'Clemson', seed: 6, region: 'West' },
    { id: 23, name: 'Dayton', seed: 7, region: 'West' },
    { id: 24, name: 'Mississippi State', seed: 8, region: 'West' },
    { id: 25, name: 'Michigan State', seed: 9, region: 'West' },
    { id: 26, name: 'Nevada', seed: 10, region: 'West' },
    { id: 27, name: 'New Mexico', seed: 11, region: 'West' },
    { id: 28, name: 'Grand Canyon', seed: 12, region: 'West' },
    { id: 29, name: 'Charleston', seed: 13, region: 'West' },
    { id: 30, name: 'Colgate', seed: 14, region: 'West' },
    { id: 31, name: 'Long Beach State', seed: 15, region: 'West' },
    { id: 32, name: 'Howard', seed: 16, region: 'West' },
    
    // South Region
    { id: 33, name: 'Houston', seed: 1, region: 'South' },
    { id: 34, name: 'Marquette', seed: 2, region: 'South' },
    { id: 35, name: 'Kentucky', seed: 3, region: 'South' },
    { id: 36, name: 'Duke', seed: 4, region: 'South' },
    { id: 37, name: 'Wisconsin', seed: 5, region: 'South' },
    { id: 38, name: 'Texas Tech', seed: 6, region: 'South' },
    { id: 39, name: 'Florida', seed: 7, region: 'South' },
    { id: 40, name: 'Nebraska', seed: 8, region: 'South' },
    { id: 41, name: 'Texas A&M', seed: 9, region: 'South' },
    { id: 42, name: 'Colorado', seed: 10, region: 'South' },
    { id: 43, name: 'Oregon', seed: 11, region: 'South' },
    { id: 44, name: 'McNeese', seed: 12, region: 'South' },
    { id: 45, name: 'Vermont', seed: 13, region: 'South' },
    { id: 46, name: 'Oakland', seed: 14, region: 'South' },
    { id: 47, name: 'Western Kentucky', seed: 15, region: 'South' },
    { id: 48, name: 'Longwood', seed: 16, region: 'South' },
    
    // Midwest Region
    { id: 49, name: 'Purdue', seed: 1, region: 'Midwest' },
    { id: 50, name: 'Tennessee', seed: 2, region: 'Midwest' },
    { id: 51, name: 'Creighton', seed: 3, region: 'Midwest' },
    { id: 52, name: 'Kansas', seed: 4, region: 'Midwest' },
    { id: 53, name: 'Gonzaga', seed: 5, region: 'Midwest' },
    { id: 54, name: 'South Carolina', seed: 6, region: 'Midwest' },
    { id: 55, name: 'Texas', seed: 7, region: 'Midwest' },
    { id: 56, name: 'Utah State', seed: 8, region: 'Midwest' },
    { id: 57, name: 'TCU', seed: 9, region: 'Midwest' },
    { id: 58, name: 'Colorado State', seed: 10, region: 'Midwest' },
    { id: 59, name: 'Virginia', seed: 11, region: 'Midwest' },
    { id: 60, name: 'James Madison', seed: 12, region: 'Midwest' },
    { id: 61, name: 'Akron', seed: 13, region: 'Midwest' },
    { id: 62, name: 'Samford', seed: 14, region: 'Midwest' },
    { id: 63, name: 'Saint Peter\'s', seed: 15, region: 'Midwest' },
    { id: 64, name: 'Grambling', seed: 16, region: 'Midwest' },
  ];

  // State for the app
  const [phase, setPhase] = useState('intro'); // intro, selection, viewBracket
  const [matchups, setMatchups] = useState([]);
  const [currentMatchupIndex, setCurrentMatchupIndex] = useState(0);
  const [bracketResults, setBracketResults] = useState({});
  const [selectedTeams, setSelectedTeams] = useState([]);
  const [swipeDirection, setSwipeDirection] = useState(null);
  const [touchStart, setTouchStart] = useState(0);
  const [touchEnd, setTouchEnd] = useState(0);

  // Create initial matchups based on seeding
  useEffect(() => {
    const createInitialMatchups = () => {
      const regions = ['East', 'West', 'South', 'Midwest'];
      let allMatchups = [];
      
      regions.forEach(region => {
        const regionTeams = initialTeams.filter(team => team.region === region);
        
        // Create first round matchups (1 vs 16, 8 vs 9, 5 vs 12, etc.)
        const seedPairs = [[1, 16], [8, 9], [5, 12], [4, 13], [6, 11], [3, 14], [7, 10], [2, 15]];
        
        seedPairs.forEach(pair => {
          const team1 = regionTeams.find(team => team.seed === pair[0]);
          const team2 = regionTeams.find(team => team.seed === pair[1]);
          
          if (team1 && team2) {
            allMatchups.push({
              id: `${region}-${pair[0]}-${pair[1]}`,
              team1: team1,
              team2: team2,
              round: 1,
              region: region
            });
          }
        });
      });
      
      // Shuffle matchups slightly to make it more interesting
      allMatchups = allMatchups.sort(() => Math.random() - 0.5);
      
      setMatchups(allMatchups);
    };
    
    createInitialMatchups();
  }, []);

  // Handle swipe events
  const handleTouchStart = (e) => {
    setTouchStart(e.targetTouches[0].clientX);
  };

  const handleTouchMove = (e) => {
    setTouchEnd(e.targetTouches[0].clientX);
  };

  const handleTouchEnd = () => {
    if (touchStart - touchEnd > 100) {
      // Swipe left
      handleSwipe('left');
    } else if (touchEnd - touchStart > 100) {
      // Swipe right
      handleSwipe('right');
    }
    
    // Reset
    setTouchStart(0);
    setTouchEnd(0);
  };

  // Handle arrow key navigation as alternative to swipes
  const handleKeyDown = (e) => {
    if (e.key === 'ArrowLeft') {
      handleSwipe('left');
    } else if (e.key === 'ArrowRight') {
      handleSwipe('right');
    }
  };

  useEffect(() => {
    document.addEventListener('keydown', handleKeyDown);
    return () => {
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [currentMatchupIndex]);

  // Handle swipe decision
  const handleSwipe = (direction) => {
    if (currentMatchupIndex >= matchups.length) return;
    
    const currentMatchup = matchups[currentMatchupIndex];
    const winner = direction === 'right' ? currentMatchup.team1 : currentMatchup.team2;
    
    setSwipeDirection(direction);
    
    // Add winner to selected teams
    setSelectedTeams(prev => [...prev, winner]);
    
    // Update bracket results
    setBracketResults(prev => ({
      ...prev,
      [currentMatchup.id]: winner
    }));
    
    // Move to next matchup
    setTimeout(() => {
      setCurrentMatchupIndex(prev => prev + 1);
      setSwipeDirection(null);
      
      // If we've gone through all matchups, create the next round
      if (currentMatchupIndex === matchups.length - 1) {
        generateNextRound();
      }
    }, 500);
  };

  // Generate the next round of matchups based on winners
  const generateNextRound = () => {
    // If we've completed all rounds, show the final bracket
    if (matchups.length > 0 && matchups[matchups.length - 1].round === 6) {
      setPhase('viewBracket');
      return;
    }
    
    const currentRound = matchups.length > 0 ? matchups[matchups.length - 1].round : 1;
    const regions = ['East', 'West', 'South', 'Midwest'];
    let newMatchups = [];
    
    if (currentRound < 4) {
      // Rounds 1-3 are within regions
      regions.forEach(region => {
        const regionResults = Object.entries(bracketResults)
          .filter(([key]) => key.startsWith(region))
          .map(([_, value]) => value);
        
        // Pair winners from previous round
        for (let i = 0; i < regionResults.length; i += 2) {
          if (i + 1 < regionResults.length) {
            newMatchups.push({
              id: `${region}-round${currentRound + 1}-${i/2}`,
              team1: regionResults[i],
              team2: regionResults[i + 1],
              round: currentRound + 1,
              region: region
            });
          }
        }
      });
    } else if (currentRound === 4) {
      // Final Four matchups (East vs West, South vs Midwest)
      const eastWinner = bracketResults[`East-round4-0`];
      const westWinner = bracketResults[`West-round4-0`];
      const southWinner = bracketResults[`South-round4-0`];
      const midwestWinner = bracketResults[`Midwest-round4-0`];
      
      newMatchups.push({
        id: 'semifinal-1',
        team1: eastWinner,
        team2: westWinner,
        round: 5,
        region: 'Final Four'
      });
      
      newMatchups.push({
        id: 'semifinal-2',
        team1: southWinner,
        team2: midwestWinner,
        round: 5,
        region: 'Final Four'
      });
    } else if (currentRound === 5) {
      // Championship game
      const finalist1 = bracketResults['semifinal-1'];
      const finalist2 = bracketResults['semifinal-2'];
      
      newMatchups.push({
        id: 'championship',
        team1: finalist1,
        team2: finalist2,
        round: 6,
        region: 'Championship'
      });
    }
    
    // Add new matchups and reset current index
    setMatchups(prev => [...prev, ...newMatchups]);
    setCurrentMatchupIndex(prev => prev);
  };

  // Render matchup card
  const renderMatchupCard = () => {
    if (currentMatchupIndex >= matchups.length) {
      return (
        <div className="flex items-center justify-center h-64 text-center">
          <div>
            <Trophy size={64} className="mx-auto mb-4 text-yellow-500" />
            <h2 className="text-2xl font-bold mb-4">Bracket Complete!</h2>
            <button 
              onClick={() => setPhase('viewBracket')}
              className="bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700"
            >
              View Your Bracket
            </button>
          </div>
        </div>
      );
    }
    
    const matchup = matchups[currentMatchupIndex];
    
    return (
      <div 
        className={`transition-transform duration-300 ${swipeDirection === 'left' ? 'translate-x-full' : swipeDirection === 'right' ? '-translate-x-full' : ''}`}
        onTouchStart={handleTouchStart}
        onTouchMove={handleTouchMove}
        onTouchEnd={handleTouchEnd}
      >
        <div className="bg-white rounded-lg shadow-lg p-6 max-w-md mx-auto">
          <div className="text-center mb-4">
            <p className="text-gray-600">{matchup.region} Region • Round {matchup.round}</p>
            <p className="text-gray-600 text-sm">Matchup {currentMatchupIndex + 1} of {matchups.length}</p>
          </div>
          
          <div className="flex justify-between items-center mb-8">
            <div className="flex-1 text-center">
              <ChevronLeft size={40} className="text-green-500 mx-auto mb-2" />
              <p className="text-sm text-gray-600">Swipe Left</p>
            </div>
            <div className="flex-1 text-center">
              <ChevronRight size={40} className="text-green-500 mx-auto mb-2" />
              <p className="text-sm text-gray-600">Swipe Right</p>
            </div>
          </div>
          
          <div className="flex justify-between items-center">
            <div className="text-center flex-1">
              <div className="bg-gray-100 rounded-md p-3 mb-2">
                <p className="text-xs text-gray-500">#{matchup.team1.seed} Seed</p>
                <h3 className="font-bold text-lg">{matchup.team1.name}</h3>
              </div>
            </div>
            
            <div className="text-center mx-4">
              <p className="text-lg font-bold text-gray-700">VS</p>
            </div>
            
            <div className="text-center flex-1">
              <div className="bg-gray-100 rounded-md p-3 mb-2">
                <p className="text-xs text-gray-500">#{matchup.team2.seed} Seed</p>
                <h3 className="font-bold text-lg">{matchup.team2.name}</h3>
              </div>
            </div>
          </div>
          
          <div className="mt-6 text-center text-gray-600">
            <p className="text-sm">Who will win this matchup?</p>
          </div>
          
          <div className="mt-6 flex justify-between">
            <button
              onClick={() => handleSwipe('right')}
              className="bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition-colors"
            >
              {matchup.team1.name}
            </button>
            <button
              onClick={() => handleSwipe('left')}
              className="bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition-colors"
            >
              {matchup.team2.name}
            </button>
          </div>
        </div>
      </div>
    );
  };

  // Render bracket view
  const renderBracketView = () => {
    const regions = ['East', 'West', 'South', 'Midwest'];
    
    return (
      <div className="mx-auto max-w-6xl">
        <h2 className="text-2xl font-bold mb-6 text-center">Your Completed Bracket</h2>
        
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          {regions.map((region) => (
            <div key={region} className="bg-white rounded-lg shadow-lg p-4">
              <h3 className="text-xl font-bold mb-4">{region} Region</h3>
              <div className="overflow-x-auto">
                <div className="flex">
                  {/* Round 1 */}
                  <div className="mr-4 space-y-2">
                    <h4 className="text-sm font-bold text-gray-500">Round 1</h4>
                    {matchups
                      .filter(m => m.region === region && m.round === 1)
                      .map(matchup => (
                        <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-32">
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team1.seed}. {matchup.team1.name}
                          </div>
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team2.seed}. {matchup.team2.name}
                          </div>
                        </div>
                      ))}
                  </div>
                  
                  {/* Round 2 */}
                  <div className="mr-4 space-y-2 mt-6">
                    <h4 className="text-sm font-bold text-gray-500">Round 2</h4>
                    {matchups
                      .filter(m => m.region === region && m.round === 2)
                      .map(matchup => (
                        <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-32 mb-12">
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team1.seed}. {matchup.team1.name}
                          </div>
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team2.seed}. {matchup.team2.name}
                          </div>
                        </div>
                      ))}
                  </div>
                  
                  {/* Sweet 16 */}
                  <div className="mr-4 space-y-2 mt-14">
                    <h4 className="text-sm font-bold text-gray-500">Sweet 16</h4>
                    {matchups
                      .filter(m => m.region === region && m.round === 3)
                      .map(matchup => (
                        <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-32 mb-28">
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team1.seed}. {matchup.team1.name}
                          </div>
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team2.seed}. {matchup.team2.name}
                          </div>
                        </div>
                      ))}
                  </div>
                  
                  {/* Elite 8 */}
                  <div className="space-y-2 mt-32">
                    <h4 className="text-sm font-bold text-gray-500">Elite 8</h4>
                    {matchups
                      .filter(m => m.region === region && m.round === 4)
                      .map(matchup => (
                        <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-32">
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold' : 'text-gray-500'}`}>
                            {matchup.team1.seed}. {matchup.team1.name}
                          </div>
                          <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font' : 'text-gray-500'}`}>
                            {matchup.team2.seed}. {matchup.team2.name}
                          </div>
                        </div>
                      ))}
                  </div>
                </div>
              </div>
            </div>
          ))}
        </div>
        
        {/* Final Four and Championship */}
        <div className="mt-8 bg-white rounded-lg shadow-lg p-4">
          <h3 className="text-xl font-bold mb-4 text-center">Final Four & Championship</h3>
          <div className="flex justify-center">
            <div className="mr-8 space-y-2">
              <h4 className="text-sm font-bold text-gray-500">Final Four</h4>
              {matchups
                .filter(m => m.round === 5)
                .map(matchup => (
                  <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-48 mb-4">
                    <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold' : 'text-gray-500'}`}>
                      {matchup.team1.seed}. {matchup.team1.name} ({matchup.team1.region})
                    </div>
                    <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font-bold' : 'text-gray-500'}`}>
                      {matchup.team2.seed}. {matchup.team2.name} ({matchup.team2.region})
                    </div>
                  </div>
                ))}
            </div>
            
            <div className="space-y-2">
              <h4 className="text-sm font-bold text-gray-500">Championship</h4>
              {matchups
                .filter(m => m.round === 6)
                .map(matchup => (
                  <div key={matchup.id} className="bg-gray-50 p-2 rounded border border-gray-200 text-sm w-48">
                    <div className={`${bracketResults[matchup.id]?.id === matchup.team1.id ? 'font-bold text-green-700' : 'text-gray-500'}`}>
                      {matchup.team1.seed}. {matchup.team1.name}
                    </div>
                    <div className={`${bracketResults[matchup.id]?.id === matchup.team2.id ? 'font-bold text-green-700' : 'text-gray-500'}`}>
                      {matchup.team2.seed}. {matchup.team2.name}
                    </div>
                    {bracketResults[matchup.id] && (
                      <div className="mt-2 text-center bg-yellow-100 p-1 rounded">
                        <Trophy size={16} className="inline-block mr-1 text-yellow-600" />
                        <span className="font-bold text-yellow-800">Champion: {bracketResults[matchup.id].name}</span>
                      </div>
                    )}
                  </div>
                ))}
            </div>
          </div>
        </div>
        
        <div className="mt-8 text-center">
          <button 
            onClick={() => {
              setPhase('intro');
              setCurrentMatchupIndex(0);
              setBracketResults({});
              setSelectedTeams([]);
              // Reset to initial state
              window.location.reload();
            }}
            className="bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700"
          >
            Start New Bracket
          </button>
        </div>
      </div>
    );
  };

  // Render intro screen
  const renderIntro = () => {
    return (
      <div className="flex flex-col items-center justify-center py-8 px-4 max-w-md mx-auto text-center">
        <Trophy size={64} className="text-yellow-500 mb-6" />
        <h1 className="text-3xl font-bold mb-4">March Madness Bracket Builder</h1>
        <p className="text-gray-600 mb-6">
          Swipe right or left to choose winners for each matchup and automatically build your bracket!
        </p>
        <button 
          onClick={() => setPhase('selection')}
          className="bg-blue-600 text-white py-3 px-6 rounded-lg hover:bg-blue-700 transition-colors text-lg font-bold"
        >
          Start Building Your Bracket
        </button>
      </div>
    );
  };

  return (
    <div className="min-h-screen bg-gray-100 p-4">
      {phase === 'intro' && renderIntro()}
      {phase === 'selection' && renderMatchupCard()}
      {phase === 'viewBracket' && renderBracketView()}
    </div>
  );
};

export default MarchMadnessBracket;